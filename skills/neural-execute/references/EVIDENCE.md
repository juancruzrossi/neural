# Risk Evidence

Use only the checks relevant to the promised behavior. Record commands and
observations in `EXECUTION.md`.

## Emergent properties

For concurrency, retry, rollback, timeout, ordering, cache, or another emergent
property, actively create the condition that could break the promise. Merely
using threads, mocks, retries, or a timer is not evidence.

For a critical atomic change:

1. Inventory observable state and fallible boundaries through publication.
2. Induce representative early and late failures, including one after valid
   work. Assert complete no-change and a successful retry.
3. Run a disposable negative control against plausible partial publication.
4. Record the boundary, failure, observed state, retry, and negative-control
   result.

Prefer preparing fallible results before the first visible mutation and
publishing once. Catch-and-restore is not proof unless fault tests also cover
the restore operations.

## Ambient state

When relevant, vary process state that callers do not pass explicitly: time,
timezone, locale, decimal precision, randomness, configuration, environment,
or cache contents. A passing default environment does not prove independence
from it.

## Scale

When input can grow, inspect loops, nested iteration, combinatorial work, I/O
per item, and shared hot paths. State the expected complexity or operational
bound and run a repeatable probe at representative scale. Compare before and
after when changing an existing path. Do not make performance claims without
measurements.

## Evidence integrity

Verify through public interfaces, not private methods, internal call order, or
storage side channels. Mock only external system boundaries. For a new guard,
observe a pass, introduce one disposable violation, observe the expected
failure, restore it, and observe a pass again.
