# Interface Design

Load this only when a plan creates or materially changes a public module
interface.

An interface is everything a caller must know: operations, inputs, invariants,
ordering, failures, configuration, and relevant performance behavior. Favor a
small caller surface that hides substantial behavior and concentrates change.
Callers and tests should use the same public interface.

Before choosing:

1. Draft at least two materially different interfaces. For each, show a short
   Python caller example, what it hides, and its failure contract.
2. Compare caller burden, change locality, testability, compatibility, and the
   number of new public interfaces.
3. Reject pass-through wrappers. If deleting the proposed module merely removes
   a name while leaving caller complexity unchanged, the interface is not
   justified.
4. Do not add an adapter for hypothetical future variation. A required public
   contract or two real behaviors that vary can justify one.
5. Record the winning interface in `PLAN.md`. Preserve rejected alternatives
   only when their trade-off warrants an ADR.
