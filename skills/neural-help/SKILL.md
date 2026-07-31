---
name: neural-help
description: "Route users through the Neural workflow and show the right starting command"
---

# Neural Help

Adapt command names to the platform: `/neural:<name>` in Claude Code and
`$<name>` in Codex. Print a concise reference. If the user describes a goal,
also name the command that starts it.

```text
Neural SDD

Start a substantial feature:
  neural-interview "<goal>"

Workflow:
  neural-interview   Clarify scope, language, decisions, and acceptance → CONTEXT.md
  neural-plan        Write a stable, shareable product specification → PLAN.md
  neural-execute     Plan just in time and implement with honest evidence → EXECUTION.md
  neural-review      Verify goal, implementation, and test quality → REVIEW.md
  neural-address-review  Apply approved fixes from a previous review.
  neural-archive     Freshness-check and archive a verified feature
  neural-learn       Rebuild project knowledge from all archives

Utility:
  neural-help        Show this reference

Flow: interview → plan → execute → review → address-review → archive → learn
```
