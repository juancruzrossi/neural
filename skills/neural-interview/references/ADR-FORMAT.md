# ADR Interface

Path: `.neural/wip/<feature>/docs/adr/NNNN-slug.md`.

Scan root and feature ADRs, then increment the highest number.

```md
# <Decision>

<Context, chosen option, and why it wins.>
```

One paragraph is often enough. Add considered options, consequences, or status
only when they preserve non-obvious information.

An ADR earns its place only when changing the decision would be expensive, its
rationale is invisible in code, and a future engineer would reasonably reopen
the trade-off. Typical cases are architectural boundaries, integration
patterns, technology lock-in, compliance constraints, and deliberate
deviations. Ordinary library choices and reversible implementation details do
not need ADRs.
