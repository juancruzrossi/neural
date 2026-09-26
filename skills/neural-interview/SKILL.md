---
name: neural-interview
description: "Interview a feature request into a shared, testable context before any plan or code exists. Use at the start of a new feature, or when scope, acceptance, or interfaces are still unclear."
---

# Neural Interview

Workflow: interview → plan → implement → review → archive. Previous: none. Next: neural-plan.

Resolve the feature name from the user's message or an existing `.neural/wip/<feature>/`; ask only if ambiguous. Normalize to kebab-case.

## Separate facts from decisions

Inspect the repo yourself for facts: git state, related code, tests, existing `.neural/wip/` or `.neural/archive/` entries. Never ask the user something the repo already answers.

Decisions are the user's: anything that changes scope, acceptance, or a public interface.

## Interview in rounds

Map open decisions as a dependency tree. Each round, ask the **frontier** — every decision whose prerequisites are already settled — as one set of numbered questions, each with a recommended answer. A question depending on an answer still open this round waits for the next round.

Cover: the problem and who has it, what "done" means, scope and non-goals, the public interface and its results, failure cases, and how it will be tested.

## Finish

Done when nothing that changes what gets built or how it is verified is still open, and the user confirms shared understanding.

Write `.neural/wip/<feature>/CONTEXT.md` with sections: Problem, Decisions, Non-goals, Acceptance criteria, Open items.

Git: if this is a git repository, commit `CONTEXT.md` locally. Never push.

Report: `Interview complete for <feature>. Context: .neural/wip/<feature>/CONTEXT.md. Next: neural-plan.`
