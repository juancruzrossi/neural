# PLAN.md Product Specification

Path: `.neural/wip/<feature>/PLAN.md`.

```md
# <Feature name>

## Summary
<What is being built and the value it creates, in a short paragraph.>

## Problem
<The current user or business problem.>

## Product Outcome
<The proposed experience and what success looks like.>

## Product Behaviors
| ID | Scenario | Expected outcome |
|---|---|---|
| B1 | <caller situation or action> | <observable result, including relevant failure state> |

## Public Interfaces
### <Interface name>
- **Operations**: <caller-visible operations>
- **Inputs**: <accepted values and invariants>
- **Outputs**: <results, ordering, identity, and side effects>
- **Failures**: <caller-visible errors and resulting state>

## Implementation Decisions
- <Consequential module, schema, compatibility, or architectural decision>

## Skills to load
<!-- Include only when --skills was provided. -->
- `<skill-identifier>`

## Testing Strategy
- **B1**: <how behavior is tested through a public interface and useful prior art>

## Acceptance Criteria
- [ ] <criterion> — B1

## Out of Scope
- <explicit exclusion>
```

Omit `## Skills to load` when `--skills` was not provided. Append `## Rollout
and Migration` only when compatibility, data movement, or release sequencing
affects the product. Append `## Delivery Constraints` only when the user
specified non-skill standards or tools that execution must preserve. Omit
optional sections when they are empty.

The specification must be stable enough to share with teammates. Do not add
exact file paths, task tables, implementation order, progress checklists,
estimates, or routine coding details. Name modules or subsystems only when the
boundary is an approved design decision.

Each behavior has one definition in `Product Behaviors`. `Testing Strategy` and
`Acceptance Criteria` must each cover every behavior ID without redefining it.
Product evidence crosses a public interface; internal inspection may diagnose a
failure but cannot close an acceptance criterion unless the internal surface is
part of the contract.

No open product question belongs in an approved specification. Return unresolved
decisions to neural-interview.
