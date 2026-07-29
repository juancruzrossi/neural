---
name: neural-execute
description: "Implement an approved product spec with just-in-time planning, honest behavior-first evidence, and a durable handoff"
---

# Neural Execute

Implement the approved behaviors in `PLAN.md`. Discover implementation details
from the current codebase instead of following a predicted file-by-file plan.

## Load the contract

Resolve the feature from `$ARGUMENTS` or `.neural/wip/`. Require `CONTEXT.md`
and `PLAN.md`; otherwise point to the missing prior phase. Read every feature
ADR. Before inspecting or changing code, load every identifier under
`Skills to load`. Preserve its recorded name and namespace; adapt only a leading
`/` or `$` to the current platform's invocation notation. If any requested skill
is unavailable, stop and report it rather than silently executing without its
guidance.

Require every product behavior to name a public interface and testing decision.
An incomplete specification returns to neural-plan.

Before changing code, read [TDD.md](./references/TDD.md) and apply every rule.

## Execute

Choose the next smallest coherent behavior or related group from the current
repository state. Inspect only then for the implementation areas and tests it
actually affects; do not predict the complete file map upfront.

For each group:

1. Apply the behavior-first loop through the specified public interface. If
   that interface cannot expose the behavior or must change, stop and return to
   neural-plan rather than testing internals or silently redesigning the spec.
2. Run focused tests during RED–GREEN. The group is complete only when each
   behavior has falsifiable evidence and its focused checks are green. For an
   atomicity or critical emergent-property promise, also require the boundary
   inventory, induced failures, and negative controls in `TDD.md`.
3. Refactor while green. Run the full suite at coherent checkpoints and always
   before handoff, plus configured build, type, and lint checks relevant to the
   actual changes.
4. Update `EXECUTION.md` with the behavior status, actual files, decisions, and
   evidence before choosing the next group.

Honor `Decision Boundaries`. Decide reversible implementation details and
record them. Stop for scope changes, new dependencies, public-contract changes,
schemas, or architectural patterns outside the approved spec. If repeated
attempts show that the specification is wrong, report the behavior blocked
instead of coding around it.

Never rewrite `PLAN.md` to match the implementation.

## Handoff

Write `.neural/wip/<feature>/EXECUTION.md` with:

- one row per behavior: status, actual files, and focused evidence;
- `RED observed` with reason, `already green`, or `N/A` for each behavior;
- for each atomicity promise, every fallible boundary, induced failure,
  observed state, retry result, and negative control;
- implementation decisions and deviations within approved boundaries;
- verification commands and results;
- blockers.

Leave all implementation and test changes local for the user to review. Never
stage files, commit, or push in any execution phase. Preserve pre-existing
staged and unrelated changes exactly as found.

Report behavior counts, deviations, local worktree state, and the
`EXECUTION.md` path.
All green: suggest `Ready to verify? Run neural-review.`
