# Neural

**A lightweight Spec-Driven Development workflow for AI coding agents.**

```text
interview → plan → implement → review → archive
```

## Install

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

### OpenCode

OpenCode has no skill installer. Paste this prompt into OpenCode:

```text
Install the Neural skills globally: clone https://github.com/juancruzrossi/neural into a temporary directory, delete every existing neural-* folder in ~/.config/opencode/skills/ (create the directory if missing), copy every folder under its skills/ directory into it, delete the temporary clone, then run `opencode debug skill` and confirm the five neural-* skills are listed.
```

## Skills

| Skill | What it does |
|---|---|
| `neural-interview` | Clarify the feature → `CONTEXT.md` |
| `neural-plan` | Write the approved plan → `PLAN.md` |
| `neural-implement` | Build the plan in vertical slices |
| `neural-review` | Verify the implementation against the plan |
| `neural-archive` | Move a passed feature to the archive |

## Artifacts

All artifacts live in `.neural/` at your project root:

```
.neural/
├── wip/
│   └── <feature>/
│       ├── CONTEXT.md
│       ├── PLAN.md
│       └── REVIEW.md
└── archive/
    └── <feature>/        moved here once REVIEW.md verdict is PASS
```
