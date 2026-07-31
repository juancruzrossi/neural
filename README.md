# Neural

**A lightweight, token-efficient Spec-Driven Development framework for AI coding agents.**

```
interview → plan → execute → review → archive → learn
```

## Why Neural?

Most AI agent failures come from unclear requirements, fantasy plans, context rot, and "done" without evidence. Neural addresses each one.

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

### Or, via skills protocol
```bash
npx skills@latest add juancruzrossi/neural
```

## Skills

| Skill | What it does |
|---|---|
| `neural-interview` | Clarify the feature → `CONTEXT.md` |
| `neural-plan` | Write the product spec → `PLAN.md` |
| `neural-execute` | Walks the plan task by task; vertical-slicing TDD |
| `neural-review` | Verifies plan vs. implementation |
| `neural-archive` | Freshness-check and archive the feature |
| `neural-learn` | Add project knowledge for future references |
| `neural-help` | Show the workflow |

## Artifacts

All artifacts live in `.neural/` at your project root:

```
.neural/
├── wip/
│   └── <feature>/
│       ├── CONTEXT.md
│       ├── PLAN.md
│       ├── EXECUTION.md
│       ├── REVIEW.md
│       └── docs/adr/     optional, consequential decisions only
├── archive/
│   └── <feature>/        freshly reviewed completed feature
└── knowledge/            built by neural-learn after each archived feature
    ├── PROJECT-CONTEXT.md   stack, conventions, recurring patterns
    ├── GLOSSARY.md          unified domain vocabulary
    ├── DECISIONS.md         cross-feature architectural decisions
    └── ANTIPATTERNS.md      recurring review findings
```
