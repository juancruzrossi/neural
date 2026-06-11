<div align="center">

# Neural

**A lightweight, token-efficient Spec-Driven Development framework for AI coding agents.**

```
interview → plan → execute → review → archive
```

</div>

## Why Neural?

Most AI agent failures aren't about bad code — they're about unclear requirements, fantasy plans, context rot, and "done" without evidence. Neural addresses each failure mode with a specific phase:

| Failure Mode | Phase | How |
|---|---|---|
| Vague requirements | Interview | Socratic questioning with selective pressure |
| Plans based on assumptions | Plan | Mandatory codebase exploration + adversarial self-review |
| Stubs and over-built code | Execute | Vertical-slice TDD loop — every line of code answers a failing test |
| "Done" but it's stubs | Review | 4-level goal-backward verification |

## Installation

### Claude Code (via Plugin Marketplace)

Register the Neural marketplace:

```
/plugin marketplace add juancruzrossi/neural
```

Then install the plugin:

```
/plugin install neural@neural
```

### Codex

Install the Neural skills globally:

```bash
curl -fsSL https://raw.githubusercontent.com/juancruzrossi/neural/main/.codex/install.sh | bash
```

## The Workflow

### `/neural:neural-interview` — Clarify before you build

Socratic interview. Clarifies requirements and captures feature context in `CONTEXT.md`.

### `/neural:neural-plan` — Plan with adversarial review

Builds a sequential task list with dependencies and per-task **Behaviors to verify** (each becomes one red→green slice). Runs a self-adversarial pass. Optional cross-review (Claude Code ⇄ Codex).

### `/neural:neural-execute` — Test-driven execution loop

Walks the plan task by task in dependency order. Vertical-slice TDD per task: failing test → minimum code → refactor on green. No stubs. Optionally, atomic commits per task.

### `/neural:neural-review` — Verify against the goal

Two layers:
1. **Plan vs Implementation** — did every task get done?
2. **Goal-Backward** — does the code solve the original problem?

### `/neural:neural-address-review` — Fix what review found

Parses REVIEW.md, builds a fix plan from blocking issues and warnings, executes fixes with verification.

### `/neural:neural-quick` — Fast-path for small tasks

Three questions, inline plan, direct execution. For small, clear tasks.

### `/neural:neural-debug` — Root-cause investigation

Systematic debugging: investigate → analyze → hypothesize → implement.

### `/neural:neural-sync` — Align specs with reality

Reads the codebase and implementation and updates CONTEXT.md and PLAN.md to match what was built. Code is the source of truth.

### `/neural:neural-status` — Where am I?

Shows progress of all features in `.neural/wip/` with next-step suggestions.

### `/neural:neural-archive` — Clean up

Moves completed features from `.neural/wip/` to `.neural/archive/`.

### `/neural:neural-help` — Command reference

Lists all Neural commands with short descriptions and the recommended workflow.

## Artifacts

All Neural artifacts live in `.neural/` at your project root:

```
.neural/
├── wip/
│   └── auth-system/
│       ├── CONTEXT.md    ← interview output
│       ├── docs/adr/     ← optional feature decisions
│       ├── PLAN.md       ← plan output
│       └── REVIEW.md     ← review output
└── archive/
    └── user-onboarding/
        ├── CONTEXT.md
        ├── docs/adr/
        ├── PLAN.md
        └── REVIEW.md
```
