---
name: neural-review
description: "Review an implementation against its plan on two axes: spec fidelity and repo standards. Use after neural-implement, before archiving a feature."
---

# Neural Review

Workflow: interview → plan → implement → review → archive. Previous: neural-implement. Next: neural-archive on PASS, neural-implement on FAIL.

Resolve the feature from the user's message or `.neural/wip/`. Require `CONTEXT.md` and `PLAN.md`.

`PLAN.md` is the claim, the code and fresh command output are the proof. Read-only on product code.

## Two axes, each with its own verdict

**Spec fidelity**: every behavior in `PLAN.md` and acceptance criterion in `CONTEXT.md` is reachable through the public interface and proven by a test that would fail if the behavior broke.

**Standards**: the change follows `AGENTS.md`/`CLAUDE.md` and repo conventions, has no speculative code, and no leftover debug residue.

Run the project's test suite and any command needed to verify a claim. Do not accept a claim without fresh evidence.

## Record and decide

Write `.neural/wip/<feature>/REVIEW.md`: Verdict (PASS/FAIL), per-axis verdict, findings with location and fix, commands run. Overall verdict is the worse of the two axes.

Git: if this is a git repository, commit locally as you finish meaningful work. Never push. Without git, just work.

Report the verdict. PASS: suggest neural-archive. FAIL: suggest neural-implement to fix the listed findings.
