---
name: neural-address-review
description: "Apply approved fixes from a previous review."
---

# Neural Address Review

Apply approved findings from `.neural/wip/<feature>/REVIEW.md` after the user approved the fix plan

## Establish scope

1. Resolve the feature from `$ARGUMENTS` or `.neural/wip/`.
2. If exactly one feature directory exists, use it automatically.
3. If several exist and no argument matches one, list them and ask which review
   to address.
4. Require `REVIEW.md`, `CONTEXT.md`, and `PLAN.md`. Read every feature ADR
   and the review's `## Findings`, `## Behavior completion`, and
   `## Goal-backward verification` sections.
5. Treat `REVIEW.md` as evidence, not as permission to expand scope. Preserve
   the approved public contract and decision boundaries.

## Build the fix plan

Classify each finding:
- **Blocking** — must be fixed before the feature can pass review.
- **Warning** — present it to the user for an explicit fix/skip decision.
- **Info** — do not change product code unless the user requests it.

Include behaviors marked `failed` or `partial` and goal-backward truths marked
`FAIL` or `PARTIAL`. Produce a concise plan that maps each approved item to
an observable correction and its verification.

Stop and show the plan. Do not modify code until the user approves it.

## Execute approved fixes

For each approved item, in dependency order:
1. Make the smallest change that addresses the finding.
2. Add or strengthen public-interface evidence when the review identified a
   coverage gap.
3. Run focused verification and then the relevant full checks.
4. Record deviations, blockers, and actual evidence locally.

Do not rewrite `PLAN.md` to match an implementation. If a fix requires a new
public contract, schema, dependency, or architectural decision, stop and return
to `neural-plan`.

## Re-review handoff

Leave all fixes and test changes local. Report:
- approved findings addressed;
- findings skipped or still blocked;
- commands and results;
- changed files;
- worktree state;
- next step: run `neural-review`.

Do not delete `REVIEW.md`; the fresh review supersedes it only after a new
`neural-review` run.
