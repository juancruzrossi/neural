---
name: neural-learn
description: "Rebuild project knowledge from verified archived features with provenance"
---

# Neural Learn

Synthesize durable project knowledge from every feature in `.neural/archive/`.
If the archive is empty, point to neural-archive and stop.

## Establish complete sources

Read existing `.neural/knowledge/` before updating it. For every archived
feature, require and read in full:

- `CONTEXT.md` — domain language and approved decisions;
- `PLAN.md` — approved product, public-interface, testing, and architectural
  decisions;
- `REVIEW.md` — Blocking and Warning findings;
- every `docs/adr/*.md`.

Do not synthesize until each archive has source coverage. A missing required
file is an error; an absent ADR directory is valid. Never infer “no findings”
without reading the review.

Archived artifacts are historical evidence. Spot-check stack, convention, and
architecture claims against the current repo. When archives disagree, prefer
current verified code; if current code cannot resolve the conflict, record it
explicitly rather than guessing which archive is newer.

## Rebuild `.neural/knowledge/`

Update all four files from the complete archive set. Existing knowledge may
preserve an explicit human conflict resolution, but unsupported or stale claims
must not survive merely because they were already present.

### `PROJECT-CONTEXT.md`

- Current verified stack and exact test/build commands.
- Structural conventions supported by evidence.
- Architectural patterns only when they recur in 2+ archived features.
- Provenance for non-obvious claims.

### `GLOSSARY.md`

- Every domain term from every archived `Language` section, with provenance.
- Definitions only: no implementation decisions.
- Surface conflicting definitions and their verified or unresolved resolution.

### `DECISIONS.md`

- Only decisions that constrain work beyond their originating feature.
- One feature is sufficient when the future impact is concrete; otherwise omit
  the decision, even on the first archive.
- Record rationale and provenance.

### `ANTIPATTERNS.md`

- Only materially equivalent Blocking or Warning findings present in 2+
  `REVIEW.md` files.
- Record category, severity, and all provenance.
- With fewer than two occurrences, write `none yet — needs 2+ features`.

Thin files after the first archive are correct. Never invent cross-feature
significance to fill them.

## Report

```text
Knowledge base updated: .neural/knowledge/
— PROJECT-CONTEXT.md
— GLOSSARY.md      (<N> terms)
— DECISIONS.md     (<N> decisions)
— ANTIPATTERNS.md  (<N> patterns)
```
