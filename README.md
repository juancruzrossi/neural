# Neural

**Spec-driven feature development for Claude Code and Codex.**

```
interview → plan → execute → review → archive → learn
```

## Why Neural?

Neural turns a feature request into a stable product spec, tested code, an
independent review, and reusable project knowledge. Implementation planning
happens just in time. Neural never stages, commits, or pushes.

## Installation

### Claude Code

```bash
claude plugin marketplace add juancruzrossi/neural
claude plugin install neural@neural
```

### Codex

```bash
codex plugin marketplace add juancruzrossi/neural
codex plugin add neural@neural
```

## Skills

| Skill | What it does |
|---|---|
| `neural-interview` | Clarify the feature → `CONTEXT.md` |
| `neural-plan` | Write the product spec → `PLAN.md` |
| `neural-execute` | Walks the plan task by task; vertical-slice TDD |
| `neural-review` | Verifies plan vs. implementation, then code vs. original goal |
| `neural-archive` | Freshness-check and archive the feature |
| `neural-learn` | Rebuild project knowledge from verified archives |
| `neural-help` | Show the workflow |

Start with `neural-interview "<goal>"`, then follow the flow.

## Artifacts

Neural creates artifacts progressively under `.neural/` at the project root:

```
.neural/
├── wip/<feature>/        CONTEXT.md · PLAN.md · EXECUTION.md · REVIEW.md
│   └── docs/adr/         optional, consequential decisions only
├── archive/<feature>/    freshly reviewed completed features
└── knowledge/            built by neural-learn after each archive
    ├── PROJECT-CONTEXT.md   stack, conventions, recurring patterns
    ├── GLOSSARY.md          unified domain vocabulary
    ├── DECISIONS.md         cross-feature architectural decisions
    └── ANTIPATTERNS.md      recurring review findings (2+ occurrences)
```
