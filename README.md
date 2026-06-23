<div align="center">

# Neural

**A lightweight, token-efficient Spec-Driven Development framework for AI coding agents.**

```
interview → plan → execute → review → archive → learn
```

</div>

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

## Commands

Invoke as `/neural:<name>` in Claude Code, or `$<name>` in Codex.

| Command | What it does |
|---|---|
| `neural-interview` | Socratic interview; captures requirements in `CONTEXT.md` |
| `neural-plan` | Sequential task list with per-task behaviors to verify; adversarial self-review, optional Claude ⇄ Codex cross-review |
| `neural-execute` | Walks the plan task by task; vertical-slice TDD, no stubs |
| `neural-review` | Verifies plan vs. implementation, then code vs. original goal |
| `neural-address-review` | Builds and executes a fix plan from `REVIEW.md` findings |
| `neural-quick` | Fast path for small tasks: three questions, inline plan, direct execution |
| `neural-debug` | Root-cause investigation: investigate, analyze, hypothesize, fix |
| `neural-sync` | Updates `CONTEXT.md` and `PLAN.md` to match the code |
| `neural-status` | Progress of every feature in `.neural/wip/` |
| `neural-archive` | Moves completed features to `.neural/archive/`; runs `neural-learn` |
| `neural-learn` | Synthesizes archived features into `.neural/knowledge/` |
| `neural-help` | Lists all commands and the recommended workflow |

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
