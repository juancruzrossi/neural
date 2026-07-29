---
name: neural-review
description: "Review implementation and test evidence against the approved feature goal, then seal the reviewed state"
---

# Neural Review

Review from fresh evidence. `PLAN.md` and `EXECUTION.md` are claims, not proof.

## Establish scope

Resolve the feature from `$ARGUMENTS` or `.neural/wip/`. Require `CONTEXT.md`
and `PLAN.md`; read `EXECUTION.md`, every feature ADR, and any skills listed in
the plan.

Identify the product and test files implicated by the actual changes. Read them,
inspect relevant wiring, and run the canonical suite plus targeted probes when
the suite cannot prove a promised property.

Identify repo standards that apply to changed files, including `AGENTS.md`,
`CLAUDE.md`, `CONTRIBUTING.md`, and local equivalents.

## Verify on two independent axes

Complete both passes. Do not let strength on one axis compensate for failure on
the other.

### Product fidelity

1. Check each specified behavior against substantive files, wiring, and fresh
   command results. Verify declared deviations rather than accepting them.
2. Derive observable truths from the product outcome and every acceptance
   criterion.
   For each truth, establish that the implementation exists, is substantive,
   is reachable through the intended public interface, and produces the
   promised outcome.

### Engineering quality

1. Check the change against applicable repo standards.
2. Audit interface design: callers and tests use the specified public interface,
   complexity is hidden rather than pushed through a shallow wrapper, and no
   speculative interface or scope was added.
3. Audit tests with the adversarial question: **could this test pass while the
   promised property is broken?** Check in particular:
   - every observable state dimension after rejected or atomic operations;
   - every reachable operation from the last validation through atomic
     publication that can fail or run caller-controlled code—including
     overloaded operations, callbacks, serialization, allocation, and I/O—
     using fault injection to prove no partial state, side effect, or
     reservation survives;
   - whether race, retry, rollback, timeout, ordering, or cache tests actively
     create the condition they claim to test;
   - a negative control for any critical emergent property without recorded
     RED evidence, using a disposable probe rather than changing reviewed files;
   - disabled, weak, circular, or implementation-coupled assertions;
   - expected values derived from an independent source.
4. Scan changed files for context-relevant incomplete work, placeholders,
   secrets, and debug residue. Confirm matches in context before reporting.

No concrete evidence means not verified.

## Record the review

Write `.neural/wip/<feature>/REVIEW.md` using
[REVIEW-FORMAT.md](./references/REVIEW-FORMAT.md).

Always include a `## Reviewed state` with SHA-256 hashes for every reviewed
product and test file. In git repos also record `HEAD` and `git status --short`
excluding `.neural/`; otherwise record `Git: unavailable`. Recompute the file
set and hashes after writing. If product state drifted, gather evidence again
before issuing a verdict.

Verdicts:

- `PASS` — complete, verified, no findings.
- `PASS WITH WARNINGS` — complete and verified, with non-blocking findings.
- `FAIL` — missing behavior or truth, failed verification, or blocking
  product-fidelity or engineering-quality issue.

The review is complete only when every behavior and acceptance criterion is
accounted for, both axes have explicit verdicts, fresh commands have finished,
every implicated product and test file is hashed, and the overall verdict is no
better than the worse axis.

## Findings

Report the verdict and next action. On request to fix, show a scoped fix plan
and wait for approval before changing product files or tests. Fix approved
findings, rerun verification, and replace the verdict only after a fresh review.

`PASS`: suggest neural-archive. `PASS WITH WARNINGS`: offer fix or explicit
acceptance. `FAIL`: do not suggest archive.

Leave reports and approved fixes local. Never stage, commit, or push; preserve
pre-existing staged and unrelated changes as found.
