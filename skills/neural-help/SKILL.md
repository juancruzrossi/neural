---
name: neural-help
description: "Show all Neural commands and the recommended workflow"
---

# Neural Help

Print the command reference below, with names adapted to the platform: `/neural:<name>` in Claude Code, `$<name>` in Codex (default to Claude Code style if unclear). Print only the reference — no extra explanation.

```text
Neural SDD — Commands

Workflow:
  neural-interview   Socratic interview → CONTEXT.md (requirements, glossary, ADRs)
  neural-plan        Sequential task plan with testable behaviors → PLAN.md
  neural-execute     Test-driven execution — one task at a time, red→green→refactor
  neural-review      Plan vs implementation + goal-backward verification → REVIEW.md
                     (fixes its own findings on request)
  neural-archive     Move the completed feature from wip/ to archive/
  neural-learn       Synthesize archived features into .neural/knowledge/

Utilities:
  neural-help        This reference

Flow: interview → plan → execute → review → archive (→ learn runs automatically)
```
