# TDD — The Vertical-Slice Loop

Tests verify **behavior through public interfaces**, not implementation details. Implementations can change entirely; tests should not. A good test reads like a specification — "user can checkout with a valid cart" — and survives internal refactors because it never looked inside.

A **seam** is the public boundary you test at. Test only at seams: prefer existing ones, and the highest one that observes the behavior.

## The loop

```
For each behavior in the task:
  RED    → write ONE test → run → confirm it fails for the right reason
           (the assertion — not a typo or missing import)
  GREEN  → write the minimum code to pass → run → confirm green
After ALL behaviors are green:
  REFACTOR → improve names, extract duplication, deepen modules → tests still green
```

- One test at a time. Only enough code to pass it — don't anticipate the next test.
- Test the behaviors the task lists; don't invent extra edge cases.
- **Never write all tests first, then all code** (horizontal slicing). Bulk tests describe *imagined* behavior, drift toward asserting shape instead of outcomes, and lock in a design before the code teaches you anything. Each cycle is a tracer bullet informed by the last.
- Never refactor while RED. Green first, always.
- On a new module the first RED cannot fail on an assertion because nothing exists yet — write a minimal skeleton (an empty function, an unregistered route) first, so the first failure is the assertion (`expected 201, got 404`), not a missing import.
- If a planned behavior is already green the first time you test it — earlier code generalized to cover it — don't fabricate a failure. Keep the test as a regression guard, note it in the report, and move on.
- After a clean run, don't re-run the same command unless code changed.

## Good and bad tests

The one question: **would this test still pass if I rewrote the internals but kept the behavior?** If no, it is coupled to implementation and will fight every refactor.

```typescript
// GOOD: observable outcome, public interface, domain language from CONTEXT.md
test("user can checkout with a valid cart", async () => {
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});

// BAD: asserts a private collaboration
test("checkout calls paymentService.process", async () => {
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

Red flags: mocking internal collaborators; asserting call counts or call order; testing private methods; verifying by bypassing the interface (e.g., raw DB query when a getter exists); restating the implementation as the expected value (`expect(add(a, b)).toBe(a + b)`) — expected values come from an independent source: a known-good literal, a worked example, the spec.

Verify through the interface, not around it:

```typescript
// BAD: bypasses the interface            // GOOD: still works if storage is rewritten
const row = await db.query("SELECT …");   const retrieved = await getUser(user.id);
expect(row).toBeDefined();                expect(retrieved.name).toBe("Alice");
```

One logical assertion per test — needing two usually means two tests.

## Mocking

Mock at **system boundaries only** — seams where your code meets something it does not own: external APIs, time and randomness, the filesystem in narrow cases, databases only when a real test DB is infeasible. Never mock your own classes, modules, or internal collaborators — those mocks couple to call shape rather than outcome, so refactors break them while real bugs pass.

Design boundaries so mocks stay trivial: pass dependencies in rather than constructing them inside, and prefer specific functions per operation (`api.getUser`, `api.createOrder`) over one generic fetcher that forces branching inside the mock.

## Refactor candidates (only on green)

Duplication → extract. Long methods → helpers, tests stay on the public interface. Shallow pass-through modules → combine or deepen (small interface, complexity hidden inside). Feature envy → move logic to its data. Primitive obsession → value object. Re-run tests after each step; if one breaks, the refactor changed behavior — revert and take a smaller step.
