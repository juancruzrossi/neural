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
ADR. Keep the skills listed under `Skills to load` in mind and load each one on
demand when its guidance is relevant to the current behavior.

Require every product behavior to map to a public interface and testing
decision. If an older plan omits the evidence mode, use the default below.
Require every non-negotiable priority in `CONTEXT.md` to survive in `PLAN.md`
as a behavior, decision, or testing gate. Other incomplete specifications
return to neural-plan.

## Execute

Choose the next smallest coherent behavior or related group from the current
repository state. When groups are similarly small, choose the one that resolves
the highest implementation uncertainty or product risk first. Inspect only then
for the implementation areas and tests it actually affects; do not predict the
complete file map upfront.

For each group:

1. Work through the specified public interface. If
   that interface cannot expose the behavior or must change, stop and return to
   neural-plan rather than testing internals or silently redesigning the spec.
2. Use the evidence mode recorded in `PLAN.md`. If none is recorded, use
   **outcome**.
   - **Outcome**: model the complete behavior group, implement it coherently,
     then add focused tests or probes through the public interface.
   - **Test-first**: write or select one behavioral test, observe a behavioral
     failure through the public interface, add the smallest coherent
     implementation, and repeat. If a new interface cannot run yet, create only
     its minimal compilable or runnable entry point first; import or setup
     failure is not behavioral RED. Never write the whole test suite before the
     implementation. If the test is already green, confirm the behavior already
     exists; never weaken code or assertions to manufacture RED. Record the
     already-green result and continue with the next unmet behavior. Refactor
     only after the behavior group is green.
3. Require falsifiable evidence for every behavior: observe the public outcome
   and promised state, derive expectations independently, and prove the check
   could fail. Read [EVIDENCE.md](./references/EVIDENCE.md) when the behavior
   promises an emergent property, uses ambient process state, or handles input
   that can grow.
4. Refactor locally after the behavior is proven. Before changing a shared or
   hot path, prove no worse asymptotic complexity with a focused benchmark or
   return the broader refactor to planning. Discover canonical verification
   commands from repository instructions, CI, wrappers, scripts, and
   configuration. Run the full suite at coherent checkpoints and always before
   handoff, plus configured build, type, and lint checks relevant to the actual
   changes.
5. Update `EXECUTION.md` with the behavior status, actual files, decisions, and
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
- evidence mode and observed result for each behavior;
- for each atomicity promise, representative early and late fallible
  boundaries, induced failure, observed state, retry result, and negative
  control;
- implementation decisions and deviations within approved boundaries;
- verification commands and results;
- blockers.

Leave all implementation and test changes local for the user to review. Never
stage files, commit, or push in any execution phase. Preserve pre-existing
staged and unrelated changes exactly as found.

Report behavior counts, deviations, local worktree state, and the
`EXECUTION.md` path.
All green: suggest `Ready to verify? Run neural-review.`
