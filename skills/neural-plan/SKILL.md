---
name: neural-plan
description: "Turn an approved interview context into an approved implementation plan with public interfaces and vertical slices. Use after neural-interview, before any code is written."
---

# Neural Plan

Workflow: interview → plan → implement → review → archive. Previous: neural-interview. Next: neural-implement.

Resolve the feature from the user's message or `.neural/wip/`. Require `CONTEXT.md`; if missing or ambiguous, send the user back to neural-interview instead of guessing.

Read related code to keep the plan realistic: existing public interfaces, domain language, and testing precedent.

## Counterexample check

Imagine two reasonable implementations that both satisfy `CONTEXT.md`. If callers could tell them apart, the plan is underspecified: return the smallest distinguishing decision to neural-interview instead of picking one.

## Write PLAN.md

Sections: Status, Summary, Behaviors table with IDs, Public interfaces, Decisions, Vertical slices (in order, each a thin end-to-end behavior with its test through the public interface), Out of scope.

No file paths, task checklists, or code snippets — those belong to implementation, not the plan.

## Approve

Show the plan and ask for explicit approval. On approval, set `Status: approved`. Do not proceed to implementation without it.

Git: if this is a git repository, commit locally as you finish meaningful work. Never push. Without git, just work.

Report: `Plan ready for <feature>. Plan: .neural/wip/<feature>/PLAN.md. Next: neural-implement.`
