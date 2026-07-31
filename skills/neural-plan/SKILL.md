---
name: neural-plan
description: "Synthesize approved discovery into a stable, shareable product spec with public contracts and testing decisions"
argument-hint: "[feature] [--skills <skill-1>, <skill-2>...]"
---

# Neural Plan

Turn approved discovery into `.neural/wip/<feature>/PLAN.md`: the stable product
specification for teammates and future agent sessions. It defines what will be
built, not an implementation checklist. Do not create a separate PRD.

## Establish the contract

1. Parse `$ARGUMENTS`. When `--skills` is provided, keep the requested skills
   in mind and load each one on demand when relevant to the current work.
2. Resolve the feature from the remaining selector or `.neural/wip/`. If none
   exists, point to neural-interview; if several exist, ask which one.
3. Require `CONTEXT.md`. Read it, every feature ADR, and `.neural/knowledge/`
   when present.
4. Inspect related code and tests to understand current behavior, domain
   language, existing public interfaces, and testing precedent. Use that
   knowledge to keep the spec realistic, not to predict an edit list.
5. Normalize each public operation and lifecycle transition into its input,
   success output, errors, next state, and observable side effects. If repeated
   statements conflict, return the exact conflict to neural-interview.
6. Run a counterexample check: imagine two reasonable implementations that
   satisfy the written context. If callers could observe different results for
   the same input or lifecycle, return the smallest distinguishing product
   decision to neural-interview.

Do not reopen resolved decisions. Implementation freedom is valid only inside
the approved decision boundaries.

## Write the specification

Use [PLAN-FORMAT.md](./references/PLAN-FORMAT.md). Make it understandable
without the interview transcript: state the problem, proposed product outcome,
observable behaviors, public contracts, consequential implementation
decisions, testing strategy, acceptance criteria, and exclusions.

`PLAN.md` becomes the canonical approved specification. Link `CONTEXT.md` and
ADRs for discovery history and rationale rather than copying the conversation.
When `--skills` was provided, include `## Skills to load` as a reminder of
the requested skills. Load them on demand when relevant. Omit the section
otherwise.

Keep decisions at product, public-interface, module, schema, or architectural
level. Do not include exact file paths, an exhaustive task breakdown, edit
order, progress tracking, or code snippets likely to become stale. A compact
prototype fragment is valid only when it expresses an approved contract more
precisely than prose. Write every code fragment in Python.

Prefer an existing public interface. When the feature creates or materially
changes one, load [INTERFACE-DESIGN.md](./references/INTERFACE-DESIGN.md) and
compare alternatives before recording the decision.

Testing decisions must observe behavior through the same public interface used
by callers or another established public interface. Internal database, private
method, filesystem, or log inspection cannot prove product behavior unless that
surface is itself part of the public contract.

Define each behavior once with an ID. Testing decisions and acceptance criteria
refer to those IDs instead of repeating the behavior. Every behavior ID must
appear in both sections; no specified behavior sits outside the release gate.
Include failure states, ordering, retries, concurrency, or rollback only where
the product promises them.

Planning is complete only when a teammate can explain what is being built and
why, behavior IDs have complete testing and acceptance coverage, every behavior
has a public way to observe it, no product ambiguity remains, and the document
contains no implementation checklist or placeholder.

## Optional adversarial review

If another supported coding agent is installed, offer one cross-review. Load
[CROSS-REVIEW.md](./references/CROSS-REVIEW.md) only if the user accepts.
Never change the specification from external feedback without explicit
approval. When declined, omit review output.

Report the behavior count and major public-interface decisions, then suggest:
`Ready to execute? Run neural-execute.`

Leave the specification local. Never stage, commit, or push; preserve
pre-existing staged and unrelated changes as found.
