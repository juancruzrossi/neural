---
name: neural-execute
description: "Test-driven execution loop — one task at a time, red→green→refactor, atomic commits"
---

# Neural Execute — Test-Driven Execution Loop

Execute the implementation plan in `PLAN.md`. Work the tasks sequentially, one at a time, in dependency order. For each task, follow the vertical-slice TDD loop in [TDD.md](./references/TDD.md): one test → minimal implementation → next test. Never batch tests, never batch implementation.

## 1. Locate the feature

Resolve the feature from `$ARGUMENTS` or `.neural/wip/` — one directory: use it; several: ask which; none: stop and point to neural-interview.

Read from `.neural/wip/<feature>/`:
- `PLAN.md` (required — stop and point to neural-plan if missing)
- `CONTEXT.md` (required — stop and point to neural-interview if missing)
- ADRs under `docs/adr/` — binding
- If `PLAN.md` has a `## Skills to load` section, load each listed skill now, before writing any code.

## 2. The loop per task

Read [TDD.md](./references/TDD.md) before the first task — it defines the red→green→refactor loop, what a good test is, and when to mock. Run tasks in table order, respecting `Depends on`.

For each behavior in the task: write ONE failing test, confirm it fails for the right reason, write the minimum code to pass, confirm green. Only when all behaviors are green: refactor, keeping tests green. Tasks with `Behaviors to verify: N/A` skip the loop — make the change and verify via build/lint as the plan states.

### Deviations

Honor the `Decision Boundaries` section of `CONTEXT.md` first — it overrides these defaults.

- **Auto-fix** (mention in report): bugs found while implementing, missing imports, build/lint errors you caused.
- **Auto-add** (flag in report): input validation, error handling for obvious failure paths, null guards on data you consume.
- **Stop and report BLOCKED**: schema changes, new external dependencies, public API changes, new architectural patterns, files outside the task's list. When unsure, do less and report — honest incomplete work beats confident wrong work.

Never rewrite `PLAN.md` to match what you built — deviations are recorded in your report, so review can still compare the implementation against the approved plan.

**Retry cap:** after 3 materially different failed attempts at one task, report it BLOCKED and stop the queue — repeated failure usually means the plan or the design is wrong. The user decides: retry, skip, or abort.

## 3. Closing a task

1. Run the full test suite; fix any regression you caused before moving on. Run build and lint if configured.
2. Record: title, status (`DONE` / `DONE_WITH_CONCERNS` — say the concern / `BLOCKED` — say what's missing), files touched, deviations.
3. Do not commit yet.
4. Print: `Progress: <done>/<total> — Task <N> "<title>": <status>`

After all tasks: sweep modified files for AI noise (comments restating code, stray debug output), then re-run the suite.

## 4. Commit phase (always ask first)

Skip entirely if `CONTEXT.md` says `**Git:** no` or there is no git repo.

Show the task → files mapping and ask: commit now (one commit per task, in order) / show full diff first / leave unstaged. If committing:

- Stage each task's files by explicit path. Never `git add -A`, `git add .`, or `--no-verify`. Exclude `.neural/**`.
- If tasks share a file, fold those tasks into a single combined commit rather than misattributing the file to the last task.
- Message: `<type>(<feature-slug>): <task-title>` — type inferred (feat / fix / refactor / chore / test / docs).
- If a commit hook fails, stop and ask: fix & retry / skip / abort.

## 5. Final report

```
Feature: <feature-name>
Tasks: <done>/<total>  (blocked/skipped: <count>)
Deviations: <list or "none">
Commits: <count>  (or "none — left unstaged")
```

All green: suggest **"Ready to verify? Run neural-review."** Anything blocked or skipped: surface it first.
