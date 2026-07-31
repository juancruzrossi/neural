---
name: neural-interview
description: "Interview a feature request into testable acceptance, domain language, decision boundaries, and selective ADRs"
---

# Neural Interview

Turn a raw feature request into a shared, testable context. Respond and write
artifacts in the user's language.

## Orient

1. Resolve the feature name and raw goal from `$ARGUMENTS`; ask only for what is
   missing. Normalize the name to kebab-case.
2. Inspect the repo for facts: git state, project context, related source and
   tests, existing ADRs, `.neural/{wip,archive}/`, and `.neural/knowledge/`.
   Established glossary and decisions are binding unless the user changes them
   explicitly. Carry relevant terms, constraints, decisions, and recurring
   warnings into the new context. Current verified code and an explicit user
   decision override stale knowledge.
3. In a multi-context repo, infer the parent context from `CONTEXT-MAP.md`; ask
   only if evidence is ambiguous.

Do not ask the user questions the repo can answer.

## Interview

Map unresolved decisions as a dependency tree. In each round, compute the
**frontier**: every decision whose prerequisites are already settled. Ask the
whole frontier together, numbered, with a recommended answer and brief
trade-off for each. A question that depends on an answer still open in this
round belongs to the next round. Do not serialize independent decisions into
one-question turns.

Defer questions that depend on answers still open in the current round. The
frontier may be smaller than the full list of decisions; completeness is
required across rounds.

Facts belong to the agent: inspect the repo or environment instead of asking.
Decisions belong to the user when they change acceptance, scope, a public
contract, or a hard-to-reverse choice. Use the interaction tools available on
the platform when they make a frontier easier to answer.

Use scenarios to expose relevant boundaries: success, failure, lifecycle,
ownership, value ranges, retries, concurrency, or time. Skip dimensions the
feature does not have.

Use judgment for reversible details inside an agreed decision boundary. Record
the choice as an assumption instead of asking for permission. Surface
contradictions with existing language, code, or decisions.

After each round, write resolved content to
`.neural/wip/<feature>/CONTEXT.md` using
[CONTEXT-FORMAT.md](./references/CONTEXT-FORMAT.md). Create the directory only
when there is resolved content to preserve.

Offer an ADR only when a decision is hard to reverse, surprising without its
rationale, and the result of a real trade-off. If accepted, use
[ADR-FORMAT.md](./references/ADR-FORMAT.md).

## Finish

Finish when the decision frontier is empty: acceptance criteria are testable,
high-impact decisions are resolved, non-goals and assumptions are visible, and
remaining external unknowns are explicit Open Items. Present the resulting
contract for one final confirmation. Do not continue interviewing for
reversible details the agent is authorized to decide.

If git is enabled and the repo is on a stable branch, ask once whether to create
an appropriate feature branch or stay. Never stage, commit, or push during the
interview; preserve pre-existing staged and unrelated changes as found.

Report:

```text
Interview complete for <feature>
Context: .neural/wip/<feature>/CONTEXT.md
ADRs: <count>
Open items: <count>
Next: neural-plan
```
