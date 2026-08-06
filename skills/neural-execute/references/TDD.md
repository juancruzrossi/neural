# Behavior-First Test Loop

Test observable behavior through the highest practical public interface.
Use the interface agreed in `PLAN.md`; test names and fixtures should use the
domain language in `CONTEXT.md`.

Before the first test run, discover the repository's canonical focused and full
verification commands from its instructions, CI, wrappers, and tool
configuration. Do not substitute a familiar command when the repository already
defines one.

For each behavior or tightly related partition:

1. **RED** — add a focused test and run it. Confirm it fails because the
   promised outcome is absent, not because of a typo or broken fixture.
2. **GREEN** — make the smallest coherent implementation change and run it.
3. **REFACTOR** — improve the design while the relevant tests remain green.

If a behavior is already green, keep the regression test when it adds evidence
and record `already green`. Never weaken code or tests to manufacture RED.
When a new module cannot import, a minimal compilable entry point is acceptable
before the first behavioral failure.

A strong test:

- observes the public result and every state dimension named by the contract;
- derives expected values independently of the implementation;
- could fail if the promised property were broken;
- survives an internal rewrite that preserves behavior.

```python
# GOOD: observable outcome, public interface, domain language from CONTEXT.md
def test_user_can_checkout_with_a_valid_cart(cart, payment_method):
    result = checkout(cart, payment_method)

    assert result.status == "confirmed"


# BAD: asserts a private collaboration
def test_checkout_calls_payment_service(cart, payment_method, mock_payment):
    checkout(cart, payment_method)

    mock_payment.process.assert_called_once_with(cart.total)
```

Verify through the interface, not around it:

```python
# BAD: bypasses the public interface
row = database.execute("SELECT * FROM users WHERE id = ?", (user.id,)).fetchone()
assert row is not None

# GOOD: survives a storage rewrite
retrieved = get_user(user.id)
assert retrieved.name == "Alice"
```

For concurrency, retry, rollback, timeout, ordering, cache, or another emergent
property, actively create the condition that could break the promise. Merely
using threads, mocks, retries, or a timer is not evidence.

When adding a verification rule—lint, import boundary, migration guard, schema
check, or similar—prove that it bites: observe a pass, introduce one disposable
violation and observe the expected failure, restore it, then observe a pass.

For a critical atomic multi-step change:

1. Inventory observable state and fallible external or caller-controlled
   boundaries from final validation through publication.
2. Induce representative early and late failures, including one after valid
   work, then assert complete no-change and a successful retry.
3. Run a disposable negative control against plausible partial publication.
4. Record one concise table in `EXECUTION.md`: boundary, failure, observed
   state, retry, and negative-control result.

Prefer preparing every fallible result before the first visible mutation and
publishing once. Catch-and-restore is not atomicity evidence unless the same
inventory and fault tests also prove that every restore operation can fail
without exposing partial state.

Avoid private-method assertions, internal call order, circular expectations,
and mocks of code the project owns. Mock only external system boundaries such
as network services, time, randomness, or an impractical real datastore.

One test may contain several assertions that jointly prove one behavior.
Several related cases may be table-driven. The unit of discipline is a
falsifiable behavior, not an arbitrary assertion count.
