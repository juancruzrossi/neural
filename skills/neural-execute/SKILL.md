---
name: neural-execute
description: "Test-driven execution loop — one task at a time, red→green→refactor, atomic commits"
---

# Neural Execute — Test-Driven Execution Loop

You are executing an implementation plan from `PLAN.md`. Work the tasks **sequentially**, one at a time, in dependency order. For each task, follow a **vertical-slice TDD loop**: one test → minimal implementation → next test. Never batch tests, never batch implementation.

The pace looks slower than batch implementation, but it is far more reliable: every line of code answers a failing test you just wrote.

## 1. Locate the feature

1. List directories under `.neural/wip/`.
2. If exactly one feature directory exists, use it automatically.
3. If multiple exist and `$ARGUMENTS` matches a feature name, use that one.
4. If multiple exist and no argument matches, list them and ask: "Which feature should I execute?"
5. Read `.neural/wip/<feature>/PLAN.md`. If missing, stop and tell the user to run `/neural:neural-plan`.
6. Read `.neural/wip/<feature>/CONTEXT.md`. If missing, stop and tell the user to run `/neural:neural-interview`.
7. Read any ADRs under `.neural/wip/<feature>/docs/adr/` — treat as binding.
8. If `PLAN.md` has a `## Model Invocable Skills` section, load each listed skill now — before building the queue or writing any code — and apply its guidance throughout the run.

## 2. Build the task queue

1. Parse the task table from `PLAN.md` — each row has number, title, dependencies, estimate.
2. Parse the "Task details" section — each task lists Behaviors to verify, Files, Acceptance.
3. Topologically sort tasks by dependencies. The result is a flat sequential queue: Task 1 → Task 2 → … → Task N.
4. If a cycle exists, stop and report it.

There are no waves. There is no parallelism. Run tasks one at a time, in the order the queue dictates.

## 3. The TDD loop per task

Before your first task, read [TDD-LOOP.md](./TDD-LOOP.md) — it defines the vertical-slice loop you follow for every task. Re-read it any time you feel tempted to write tests in bulk or skip the red step.

```
For each behavior listed in the task:
  RED    → write ONE test that asserts the behavior → run → confirm it fails for the right reason
  GREEN  → write the minimum code to pass that test → run → confirm green
  (only after all behaviors green for this task)
  REFACTOR → improve names, extract duplication, deepen modules → run tests → still green
```

Three companion references — read each when its situation arises:

- [TDD-TESTS.md](./TDD-TESTS.md) — when a test feels coupled to implementation (assert behavior through the public interface, not internal calls or shape).
- [TDD-MOCKING.md](./TDD-MOCKING.md) — when you reach for a mock (mock only at system boundaries; never your own modules).
- [TDD-DESIGN.md](./TDD-DESIGN.md) — when refactoring or shaping an interface (deep modules, refactor candidates).

If the task has no testable behavior (e.g., pure config, formatter rules, dependency bump), skip the loop and just make the change. Verify by running the build or lint. Note in the report that TDD was N/A.

## 4. Inside a task — handling surprises

You will encounter things the task did not anticipate. Read [TASK-PROTOCOL.md](./TASK-PROTOCOL.md) for the deviation rules (auto-fix vs auto-add vs ask) and status protocol. The summary: prefer doing less and reporting BLOCKED over silently expanding scope.

Honor the "Decision Boundaries" section in `CONTEXT.md` — those are feature-specific rules that override the generic protocol.

**Retry cap:** after 3 materially different failed attempts at the same task (different approaches — not the same one retried), stop and report it BLOCKED. Repeated failure usually means the plan or the design is wrong; don't keep adding risk.

## 5. Closing a task

When every behavior is green and refactors are done:

1. Run the full project test suite (if one exists). If anything outside this task broke, fix the regression before moving on.
2. Run the build (if configured) and linter (if configured). Resolve issues.
3. Record the task outcome: title, status (`DONE` / `DONE_WITH_CONCERNS` / `BLOCKED`), files touched (modified + created + deleted), and any concerns.
4. **Do not commit yet.** All commits happen in § 7 under user approval.

If the task is `BLOCKED`, stop the queue and report. Do not start the next task until the user decides: retry, skip (with downstream impact noted), or abort.

## 6. Progress and cleanup

After each task completes, print a one-line update:

```
Progress: <done>/<total> — Task <N> "<title>": <status>
```

After all tasks complete:

1. Sweep for AI noise on modified files: comments restating obvious code, stray `console.log` / `print` / `debugger`, over-documented obvious methods.
2. Re-run the test suite after cleanup. If something breaks, revert the cleanup for that file.

## 7. Commit phase (final step — always ask)

Skip this entire step if `CONTEXT.md` has `**Git:** no` or `git rev-parse --is-inside-work-tree` fails.

The orchestrator is the **single git writer**. No commits happened during the loop. Now propose one commit per task, in task-number order, with explicit user approval.

1. Print the accumulated task → files mapping:
   ```
   Changes ready to commit (feature <feature-name>):
     Task 1 — <title>: <file1>, <file2>
     Task 2 — <title>: <file3>
     ...
   Total: N tasks, M files, working tree unstaged.
   ```
2. Ask:
   > "All tasks completed. Progressively commit, one commit per task, in task-number order?"
   > 1. Yes — commit now.
   > 2. Show full diff first (`git diff` + per-task file list), then decide.
   > 3. No — leave unstaged; I'll handle commits manually.
3. **If Yes** — for each `DONE` / `DONE_WITH_CONCERNS` task, in task-number order:
   a. Stage only that task's files by explicit path: `git add <file1> <file2> ...`. Never `git add -A`, `git add .`, or `git commit -am`. Exclude `.neural/**` and files owned by other tasks.
      - If a file appears in multiple tasks, assign it to the last task that touched it.
   b. Run `git diff --cached --name-only` and confirm the staged set matches the task's file list. If it drifts, stop and report.
   c. Commit with `<type>(<feature-slug>): <task-title>` (type inferred: feat / fix / refactor / chore / test / docs).
   d. Record the commit hash.

   After the loop: `git log --oneline <base>..HEAD` and confirm commit count equals successful task count. Surface any divergence.

   If `git commit` fails (hook rejects, hook auto-fixes and re-dirties the tree), stop and ask: fix & retry / skip that task / abort. Never `--no-verify`.
4. **If Show diff** — print `git status` + per-task file list, then re-prompt from step 2.
5. **If No** — note the working tree is left unstaged; the user owns commits from here.

## 8. Final report

Print:

```
Feature: <feature-name>
Tasks completed: <count>
Tasks blocked/skipped: <count>
Files changed: <count>
Commits created: <count>  (or "none — left unstaged")
```

If everything succeeded, suggest: **"Ready to verify? Run `/neural:neural-review`."**
If anything blocked or was skipped, surface it before suggesting review.
