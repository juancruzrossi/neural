---
name: neural-address-review
description: "Apply approved fixes from a previous review."
---

# Neural Address Review

Apply user-approved findings from a final or adversarial execution review.

## Establish scope

1. Resolve the feature from `$ARGUMENTS` or `.neural/wip/`.
2. If exactly one feature directory exists, use it automatically.
3. If several exist and no argument matches one, list them and ask which review
   to address.
4. Require `EXECUTION.md`, `CONTEXT.md`, and `PLAN.md`, plus at least one finding
   source: `REVIEW.md` or `adversarial-review/EXECUTION-REVIEW.md`. Read every
   feature ADR, any skills listed in the plan, and only the source findings the
   user selected. Load listed skills on demand when relevant to a fix.
5. Keep the source path attached to every finding. Treat either review as
   evidence, not as permission to expand scope. Never merge adversarial
   findings silently into `REVIEW.md`. Preserve the approved public contract
   and decision boundaries.

## Build the fix plan

Classify each finding:
- **Blocking** — must be fixed before the feature can pass review.
- **Warning** — present it to the user for an explicit fix/skip decision.
- **Info** — do not change product code unless the user requests it.

For `REVIEW.md`, include contract rows marked `FAIL` or `PARTIAL`. For either
source, include only findings the user approved. Produce a concise plan that
maps each approved item and its provenance to an observable correction and its
verification.

Stop and show the plan. Do not modify code until the user approves it.

## Execute approved fixes

For each approved item, in dependency order:
1. Make the smallest change that addresses the finding.
2. Add or strengthen public-interface evidence when the review identified a
   coverage gap.
3. Run focused verification and then the relevant full checks.
4. Append the approved correction, actual files, and evidence to
   `EXECUTION.md`. Preserve its prior history.
5. Record deviations and blockers locally.

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
