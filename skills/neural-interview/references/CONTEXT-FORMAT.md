# CONTEXT.md Interface

Path: `.neural/wip/<feature>/CONTEXT.md`.

Required sections preserve the contract between interview, plan, execute, and
review. Optional sections should appear only when they contain useful evidence.

```md
# <Feature>

**Git:** yes|no
**Branch:** <branch>
**Parent context:** <name> <!-- only for multi-context repos -->

## Problem
<What must change and why.>

## Language <!-- optional -->
**<Domain term>**:
<Definition of what it is.>
_Avoid_: <aliases>

## Decisions
- <Resolved product or technical choice>

## Constraints <!-- optional -->
- <Limit imposed by product, platform, policy, or compatibility>

## Scenarios <!-- optional -->
- <Representative flow or edge case>

## Non-goals
- <Explicitly excluded outcome>

## Assumptions <!-- optional -->
- <Reversible choice made by the agent, open to correction>

## Decision Boundaries
- Agent may decide: <reversible, low-risk choices>
- Ask user before: <scope, contract, irreversible, or high-risk choices>

## Acceptance Criteria
- [ ] <Observable, testable outcome>

## Open Items <!-- optional -->
- <Unresolved question and why it remains unresolved>
```

`Problem`, `Decisions`, `Non-goals`, `Decision Boundaries`, and `Acceptance
Criteria` are required. Acceptance must cover the observable success path and
each relevant failure or side effect. Add concurrency, retry, timing, or
rollback criteria only when the feature promises those properties.

`Language` is a strict domain glossary: no implementation details or generic
programming terms. Choose one canonical term and list meaningful aliases under
`_Avoid_`.
