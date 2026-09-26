---
name: neural-implement
description: "Implement an approved plan one vertical slice at a time, test first through the public interface. Use after a plan is approved, or to fix FAIL findings from neural-review."
---

# Neural Implement

Workflow: interview → plan → implement → review → archive. Previous: neural-plan. Next: neural-review.

Resolve the feature from the user's message or `.neural/wip/`. Require `PLAN.md` with `Status: approved`; otherwise stop and send the user to neural-plan.

If `.neural/wip/<feature>/REVIEW.md` exists with FAIL findings, fix only those findings — no new scope — then go to Before handoff.

## Build in vertical slices

Take the plan's slices in order. For each: write a failing test through the public interface, write the smallest code across every layer to pass it, then move to the next slice. Refactor only while the suite is green.

Stop and ask before a scope change, a new dependency, or a public-contract change. Never rewrite `PLAN.md` to match the implementation.

## Before handoff

Run the project's own tests, types, and lint, then the full suite.

Git: if this is a git repository, commit locally as you finish meaningful work. Never push. Without git, just work.

Report the slices done and any deviations. Suggest neural-review.
