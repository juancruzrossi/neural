---
name: neural-interview
description: "Socratic interview that captures domain language, decisions, and ADRs inside .neural/wip/<feature>/"
---

# Neural Interview — Clarify Before You Build

> **Never use the `AskUserQuestion` tool.** One question at a time, in plain prose, waiting for the user's free-form answer — no multiple-choice, ever.

Interview the user relentlessly about every aspect of the feature until you reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer. If a question is a matter of fact about the existing code (stack, naming, current patterns), explore instead of asking — but never infer a design decision, requirement, or the user's intent from how things work today; ask those.

Respond in the user's language. Write `CONTEXT.md` and ADRs in that same language.

## Before pressing

1. Check git silently: `git rev-parse --is-inside-work-tree 2>/dev/null`.
2. Ask the feature name; normalize to kebab-case. Ask for the raw description.
3. Scan for existing context: `CONTEXT-MAP.md` (multi-context) or root `CONTEXT.md` (single-context), `docs/adr/`, related source and tests, and `.neural/{wip,archive}/*/CONTEXT.md`.
4. If `.neural/knowledge/` exists, read it — treat its glossary terms as established and decisions as binding.
5. If `CONTEXT-MAP.md` exists, infer which bounded context this feature belongs to; ask only if unclear.

Create files lazily — only when there is something to write. Don't create `.neural/wip/<feature>/` until the first section is ready.

## During the session

Challenge the glossary: when a term conflicts with existing language in `CONTEXT.md`, call it out before moving on — "your glossary defines X as Y, but you mean Z — which is it?"

Sharpen fuzzy language: when a term is vague or overloaded, propose one canonical name and reject the rest. Be opinionated.

Stress-test relationships: **invent** concrete scenarios that probe edge cases and force the user to be precise about the boundaries between concepts — ownership, cardinality, lifecycle.

Cross-reference with code: when the user states how something works, check the code agrees. Surface contradictions.

Surface assumptions as you go: when you catch yourself inferring an unstated requirement, state it inline as an assumption and invite correction — don't wait for the end.

Update `CONTEXT.md` inline as terms resolve — don't batch. Its `Language` section is a strict glossary — no implementation details, no decisions, no acceptance criteria (those have their own sections). The document as a whole is a lightweight context capture, not a full spec. Use [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

Offer an ADR only when all three hold: hard to reverse, surprising without context, the result of a real trade-off. Otherwise skip it. Use [ADR-FORMAT.md](./ADR-FORMAT.md).

## Finish

List the assumptions *and the non-goals* you inferred out loud; if the user corrects any, update `CONTEXT.md`.

If on a stable branch (`main`, `master`, `develop`, `stage`, `staging`, `production`, `release`), ask whether to create `feature/<slug>`, `enhancement/<slug>`, `fix/<slug>`, `hotfix/<slug>`, or stay.

> **Git:** By default the `.neural/` directory stays out of version control — commit it only if the user explicitly asks

Report:

```text
Interview complete for <feature>
Context: .neural/wip/<feature>/CONTEXT.md
ADRs: <count>
Open items: <count>
Next: /neural:neural-plan
```
