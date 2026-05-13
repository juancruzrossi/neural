# ADR Format

Path: `.neural/wip/<feature>/docs/adr/NNNN-slug.md`.

Numbering: scan existing `docs/adr/` (root and feature), take the highest number, increment.

## Template

```md
# <Decision>

<1-3 sentences: context, decision, why.>
```

## When to write one

All three must hold:

1. **Hard to reverse** — changing your mind later costs real effort.
2. **Surprising without context** — a future reader will wonder "why?"
3. **The result of a real trade-off** — genuine alternatives existed.

Skip otherwise.
