# REVIEW.md Interface

```md
# Review: <feature>

## Verdict: PASS | PASS WITH WARNINGS | FAIL
<Short rationale.>

| Axis | Verdict | Worst finding |
|---|---|---|
| Product fidelity | PASS/WARN/FAIL | <finding or none> |
| Engineering quality | PASS/WARN/FAIL | <finding or none> |

## Evidence
- <files, ADRs, commands, and targeted probes actually inspected>

<!-- Include this section only when --skills was provided. -->
## Model Invocable Skills
- `<skill-identifier>`

## Contract verification
| Behavior / truth | Implementation evidence | Test/runtime evidence | Status |
|---|---|---|---|
| <identity> — <observable truth> | <path/symbol> | <test/probe/result> | PASS/PARTIAL/FAIL |

**Score:** <completed>/<total>

## Test-quality audit
- <finding or "None">

## Findings
### Blocking
- <finding or "None">
### Warnings
- <finding or "None">
### Info
- <finding or "None">

## Reviewed state
Git: <HEAD and status | unavailable | disabled by context>
Files reviewed:
- `<path>` — product or test

## Next action
<one safe next step>
```

Every acceptance criterion must map to a truth. Cite concrete paths, symbols,
tests, standards, or command outcomes; summaries without evidence are
insufficient. The overall verdict cannot be better than the worse axis.
