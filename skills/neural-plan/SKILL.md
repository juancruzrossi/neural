---
name: neural-plan
description: "Implementation planning from CONTEXT.md — sequential vertical-slice tasks, each carrying testable behaviors, with optional adversarial cross-review (Claude Code ⇄ Codex)"
argument-hint: "[feature]"
---

# Neural Plan — Implementation Planning

Generate an implementation plan from the feature `CONTEXT.md` produced by interview.

## 1. Locate the feature

Resolve the feature from `$ARGUMENTS` or `.neural/wip/` — one directory: use it; several: ask which; none: stop and point to neural-interview.

Read `.neural/wip/<feature>/CONTEXT.md` (required — stop and point to neural-interview if missing) and any ADRs under `.neural/wip/<feature>/docs/adr/` — treat them as binding.

## 2. Ground the plan in the codebase

1. If `.neural/knowledge/` exists, read it first — its stack, conventions, decisions, and anti-patterns are established facts.
2. Read the source related to the feature — models, routes, components, tests — and note the existing patterns to follow.
3. Detect the test runner and its canonical command (e.g., `pnpm test`, `pytest -q`); execute needs it.
4. If `CONTEXT.md` contradicts the code, stop and ask the user to resolve it.

## 3. Generate PLAN.md

Write `.neural/wip/<feature>/PLAN.md`:

```markdown
# Plan: <feature-name>

## Overview
<!-- 2-3 sentences: what will be built and why -->

## Test Runner
<!-- The exact command. "none detected" if applicable. -->

## Skills to load
<!-- Only if the user asked for specific skills to shape this work: list each one
     with a one-line reason. Omit otherwise. -->

## Tasks

| # | Task | Depends on |
|---|------|-----------|
| 1 | ... | — |
| 2 | ... | 1 |

### Task 1: <title>
- **What**: concrete deliverable
- **Files**: the files this task will touch
- **Behaviors to verify**: observable, testable statements — each becomes one red→green slice.
  - e.g., "Submitting a valid login returns a JWT cookie."
  - e.g., "Invalid credentials return 401 without a cookie."
- **Acceptance**: how to know this task is done (usually: all behaviors green + build/lint clean).

<!-- Repeat per task -->

## Acceptance Criteria
- [ ] <!-- Derived from CONTEXT.md and feature ADRs. -->
```

Rules:

- Tasks are sequential, atomic vertical slices, numbered from 1, dependencies explicit.
- Every task lists Behaviors to verify. Tasks with nothing testable (pure config, dependency bumps) write `Behaviors to verify: N/A — non-testable change` and say how they will be verified instead (build, lint, manual check).
- **No placeholders.** Banned: "TBD", "TODO", "implement later", "add appropriate error handling" (specify what), "similar to Task N" (spell it out), "add necessary tests" (the Behaviors list IS the test list). If you cannot be specific, the context needs more detail — send the user back to neural-interview.

## 4. Optional cross-review

Offer an adversarial review from the *other* agent — Claude Code ⇄ Codex:

1. Check the other agent is installed (`codex --version` / `claude --version`). Missing: skip silently to step 5.
2. Ask: **"<other agent> is available. Send this plan for adversarial review?"** Declined: skip to step 5.
3. Run it — feed the prompt on stdin, pass file references (never inline content), and read the review from the output file, not stdout:

   ```bash
   # Claude Code → Codex:
   mkdir -p /tmp/.neural
   codex exec --ephemeral -C "$PWD" -s read-only -o /tmp/.neural/<feature>-review.md - <<'PROMPT'
   ...
   PROMPT

   # Codex → Claude Code:
   mkdir -p /tmp/.neural
   claude --print --no-session-persistence --allowedTools "Read,Grep,Glob" > /tmp/.neural/<feature>-review.md <<'PROMPT'
   ...
   PROMPT
   ```

   Prompt body: adversarial reviewer for <project> (<stack>); review the plan against the context and ADRs — critical issues, missing edge cases, dependency gaps, and behaviors coupled to implementation rather than observable through the public interface; do not invent files or symbols — say "unverified" when unsure; cite task numbers; review only, apply nothing. Reference `@.neural/wip/<feature>/CONTEXT.md`, `@.neural/wip/<feature>/PLAN.md`, `@.neural/wip/<feature>/docs/`.

4. Show the full review and ask: apply all / cherry-pick / ignore. Never modify the plan without explicit approval.

## 5. Finalize

Print a summary — task count, total behaviors — and suggest: **"Ready to execute? Run neural-execute."**
