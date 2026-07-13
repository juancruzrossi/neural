<div align="center">

# Neural

**A lightweight, token-efficient Spec-Driven Development framework for AI coding agents.**

```
interview → plan → execute → review → archive → learn
```

</div>

## Why Neural?

Most AI agent failures come from unclear requirements, fantasy plans, context rot, and "done" without evidence. Neural addresses each one — with the minimum process a modern model actually needs, and nothing else.

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

## Commands

Invoke as `/neural:<name>` in Claude Code, or `$<name>` in Codex.

| Command | What it does |
|---|---|
| `neural-interview` | Socratic interview; captures requirements in `CONTEXT.md` |
| `neural-plan` | Sequential task list with per-task behaviors to verify; optional adversarial Claude ⇄ Codex cross-review |
| `neural-execute` | Walks the plan task by task; vertical-slice TDD, no stubs |
| `neural-review` | Verifies plan vs. implementation, then code vs. original goal; fixes findings on request |
| `neural-archive` | Moves completed features to `.neural/archive/`; runs `neural-learn` |
| `neural-learn` | Synthesizes archived features into `.neural/knowledge/` |
| `neural-help` | Shows all commands and the recommended workflow |

## Artifacts

All Neural artifacts live in `.neural/` at your project root:

```
.neural/
├── wip/<feature>/        CONTEXT.md · docs/adr/ · PLAN.md · REVIEW.md
├── archive/<feature>/    same layout, completed
└── knowledge/            built by neural-learn after each archive
    ├── PROJECT-CONTEXT.md   stack, conventions, recurring patterns
    ├── GLOSSARY.md          unified domain vocabulary
    ├── DECISIONS.md         cross-feature architectural decisions
    └── ANTIPATTERNS.md      recurring review findings (2+ occurrences)
```

## Migrating from 1.x

2.0 cuts the framework to its load-bearing core. Removed commands and where their job went:

| Removed | Replacement |
|---|---|
| `neural-quick` | Just ask your agent — the fast path is not invoking Neural |
| `neural-status` | Ask your agent to inspect `.neural/wip/` |
| `neural-debug` | Just ask your agent to debug — models do this well by default |
| `neural-sync` | Execute reports deviations; archived specs are historical documents |
| `neural-address-review` | `neural-review` fixes its own findings on request |
| `neural-plan --visual` | Removed |
| `neural-plan --skills` | Ask for skills in plain words; the plan records them under `Skills to load` |
