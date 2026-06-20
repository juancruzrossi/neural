---
name: neural-plan
description: "Implementation planning with adversarial review and optional cross-review (Claude Code ⇄ Codex). Tasks are sequential vertical slices, each carrying its own testable behaviors"
---

# Neural Plan — Implementation Planning

You are generating an implementation plan from the feature `CONTEXT.md` produced by interview. The plan feeds a test-driven execution loop, so every task must declare the **behaviors** it will deliver. Each behavior becomes one red→green slice in `/neural:neural-execute`.

## 1. Locate the feature context

1. List directories under `.neural/wip/`.
2. If exactly one feature directory exists, use it automatically.
3. If multiple exist and `$ARGUMENTS` matches a feature name, use that one.
4. If multiple exist and no argument matches, list them and ask: "Which feature should I plan?"
5. Read `.neural/wip/<feature>/CONTEXT.md`. If missing, stop and tell the user to run `/neural:neural-interview`.
6. Read any ADRs under `.neural/wip/<feature>/docs/adr/` — treat as binding.

## 1b. Explore the codebase

Before writing any plan, ground tasks in reality:

1. If `.neural/knowledge/` exists, read all four files first — treat stack facts, conventions, and patterns as established, decisions as binding, and anti-patterns as constraints to plan around.
2. Scan the project structure — frameworks, patterns, conventions.
3. Read files related to the feature — existing models, routes, components, tests.
4. Identify dependencies and integration points.
5. Note existing patterns to follow (naming, folder structure, error handling).
6. Cross-check feature language against existing code. If `CONTEXT.md` contradicts the code, stop and ask the user to resolve it.
7. Detect the test runner (e.g., `vitest`, `jest`, `pytest`, `go test`) and note the canonical command. The execute phase will need it.

A plan grounded in the actual codebase is dramatically more executable than one based on guesswork.

## 2. Generate the plan (single pass)

Produce `.neural/wip/<feature>/PLAN.md` with this structure:

```markdown
# Plan: <feature-name>

## Overview
<!-- 2-3 sentences: what will be built and why -->

## Test Runner
<!-- The exact command the executor should run (e.g., `pnpm test`, `pytest -q`). State "none detected" if applicable. -->

## File Map

| File | Action | Responsibility |
|------|--------|---------------|
| src/auth/login.ts | Create | Login handler with JWT |
| src/auth/middleware.ts | Modify | Add token validation |

<!-- ALL files touched, with purpose. Prevents tasks from overlapping or forgetting files. -->

## Tasks

| # | Task | Depends on | Estimate |
|---|------|-----------|----------|
| 1 | ... | — | S/M/L |
| 2 | ... | 1 | S/M/L |

### Task details

#### Task 1: <title>
- **What**: concrete deliverable
- **Why**: how it advances the feature
- **Files**: the files this task will touch
- **Behaviors to verify**: bullet list of observable, testable statements. Each one becomes a red→green slice during execution.
  - e.g., "Submitting a valid login returns a JWT cookie."
  - e.g., "Invalid credentials return 401 without a cookie."
- **Acceptance**: how to know this task is done (usually: all behaviors have passing tests + build/lint green).

<!-- Repeat for each task -->

## Risk Assessment

| Risk | Impact | Likelihood | Mitigation |
|------|--------|-----------|------------|
| ... | H/M/L | H/M/L | ... |

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
<!-- Derived from CONTEXT.md acceptance criteria and ADRs. -->
```

Rules for task generation:

- Number tasks sequentially from 1.
- Each task is **atomic** — one clear deliverable.
- Declare dependencies explicitly (task numbers, or `—` for none).
- Estimate: S (< 30 min), M (30-120 min), L (> 2 hrs).
- Each task **must** list Behaviors to verify. The executor turns each into a single test. Tasks without testable behavior (pure config, formatter rules, dependency bumps) must say so explicitly — write `Behaviors to verify: N/A — non-testable change` and explain how it will be verified instead (build, lint, manual check).
- Derive acceptance criteria directly from `CONTEXT.md` and feature ADRs.

**No placeholders.** Every task must contain concrete, specific values. Banned phrases:

- "TBD", "TODO", "implement later", "to be determined"
- "add appropriate error handling" (specify what)
- "similar to Task N" (spell it out)
- "align X with Y" (state the concrete target)
- "add necessary tests" (the Behaviors list IS the test list — be specific)

If you cannot be specific, the feature context needs more detail — send the user back to `/neural:neural-interview`.

## 3. Adversarial self-review

After writing the plan, do a second pass and answer:

1. **Missing edge cases** — inputs, states, or flows the tasks do not cover?
2. **Dependency gaps** — could any task fail because a predecessor is incomplete?
3. **Scope creep** — any task exceeds what `CONTEXT.md` asks for?
4. **Scope reduction (BLOCKING)** — does any task *reference* a `CONTEXT.md` decision but deliver a reduced version of it? Scan tasks and behaviors for: "v1", "for now", "hardcoded", "placeholder", "stub", "wired later", "basic version", "too complex". If found, the task must deliver the decision in full or be split into explicit phases — never silently shrink the requirement.
5. **Behavior coverage** — does every requirement from `CONTEXT.md` and the ADRs appear as a behavior in at least one task? List any uncovered requirements.
6. **Rollback** — if a task fails partway, can we revert cleanly?
7. **Behavior quality** — is each "Behavior to verify" observable through the public interface, or does it leak implementation? Rewrite leaky ones.

Append findings:

```markdown
## Adversarial Review

### Issues found
- ...

### Adjustments made
- ...
```

Update tasks and behaviors if the review surfaces real issues. Note what changed.

## 4. Optional cross-review

After writing PLAN.md, offer an adversarial review from the *other* agent — Claude Code ⇄ Codex:

1. Check it's installed (`codex --version` / `claude --version`). If missing, skip silently and go to step 5.
2. Ask: **"<other agent> is available. Send this plan for adversarial review?"** If declined, go to step 5.
3. Run it — swap the command for your runtime. Feed the prompt on stdin, never as an `argv` string. Pass **file references**, never inline content:

   ```bash
   # Claude Code → Codex:
   mkdir -p /tmp/.neural
   codex exec --ephemeral -C "$PWD" -s read-only \
     -o "/tmp/.neural/<feature>-codex-review.md" - <<'PROMPT'
   <prompt below>
   PROMPT
   # then read the clean review FROM THE FILE (not stdout):
   cat "/tmp/.neural/<feature>-codex-review.md"

   # Codex → Claude Code:
   mkdir -p /tmp/.neural
   claude --print --no-session-persistence --allowedTools "Read,Grep,Glob" \
     > "/tmp/.neural/<feature>-claude-code-review.md" <<'PROMPT'
   <prompt below>
   PROMPT
   # then read the clean review FROM THE FILE (not stdout):
   cat "/tmp/.neural/<feature>-claude-code-review.md"
   ```

   Prompt (the heredoc body):

   ```
   You are an adversarial reviewer for <project-name> (<tech stack>).

   Review the implementation plan against the feature context and ADRs. Find critical issues, missing edge cases, architectural risks, dependency gaps. Pay special attention to the Behaviors to verify — flag any coupled to implementation rather than observable through the public interface.

   Grounding: do not invent files, symbols, or code paths you cannot point to in the provided files. If unsure, say "unverified" — a fabricated issue is worse than a missed one. Prefer one well-grounded finding over several speculative ones.

   Relevant files:
   @CLAUDE.md
   @AGENTS.md
   @.neural/wip/<feature>/CONTEXT.md
   @.neural/wip/<feature>/PLAN.md
   @.neural/wip/<feature>/docs/

   Output a structured review with CRITICAL issues, WARNINGS, and SUGGESTIONS. Cite task numbers. This is review only — do not apply changes yourself; the orchestrator decides what to adopt.
   ```

4. Show the full review — read it from `/tmp/.neural/<feature>-<adversarial-agent>-review.md` (the reviewer's file), not from stdout — and ask:

   > "Review above. What do you want to do?"
   > 1. Apply all — I update the plan
   > 2. Cherry-pick — tell me which
   > 3. Ignore — keep as-is

   Do **not** modify the plan without explicit approval. If changes are applied, append:

   ```markdown
   ## Cross-Review
   <!-- Reviewer feedback and user-approved changes -->
   ```

## 5. Finalize

1. Write the final PLAN.md to `.neural/wip/<feature>/PLAN.md`.
2. Print a summary: task count, total behaviors, top risks.
3. Suggest: **"Ready to execute? Run `/neural:neural-execute`."**
