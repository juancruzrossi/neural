---
name: neural-learn
description: "Harvest knowledge from archived features into .neural/knowledge/ — run after archiving a feature, or invoke manually to rebuild the project knowledge base"
---

# Neural Learn — Harvest and Synthesize

## Steps

1. **Check the archive.** List all feature directories in `.neural/archive/`. If none exist, stop: "No archived features yet. Run neural-archive after completing a feature."

   Archived artifacts are historical documents — they describe the code as it was when the feature shipped. When two features conflict, the newer one wins; before recording a stack or convention fact, spot-check it against the current repo.

2. **Load existing knowledge.** Read `.neural/knowledge/` in full if it exists — merge, never overwrite.

3. **Harvest from every archived feature.**

   For each directory in `.neural/archive/*/`, read:
   - `CONTEXT.md` — `Language` section (domain terms) and feature decisions
   - `PLAN.md` — tech stack signals: test runner command, frameworks, file structure patterns
   - `REVIEW.md` — every Blocking and Warning finding
   - `docs/adr/*.md` — architectural decisions

4. **Synthesize into `.neural/knowledge/`.** Write or update all four files. Every archived feature must be reflected before this step is complete.

   **First feature (n=1):** the cross-feature thresholds below leave most files thin — that's correct, not a failure. Fill `GLOSSARY.md` and the stack facts in `PROJECT-CONTEXT.md`; record decisions and recurring-pattern candidates as single-provenance; write `ANTIPATTERNS.md` as `none yet — needs 2+ features`. Never invent cross-feature impact to fill a file.

   **`PROJECT-CONTEXT.md`** — project-level facts every feature inherits:
   - Tech stack: language and supported runtime version from manifests, framework, test runner with exact command
   - Structural conventions: naming patterns, folder layout, import style derived from the archive
   - Architectural patterns that recur across 2+ features

   **`GLOSSARY.md`** — unified domain vocabulary:
   - One entry per term, merged from every `Language` section in every `CONTEXT.md`
   - Conflicts surface explicitly: `⚠️ Conflict: [feature-a] defines as X; [feature-b] defines as Y — resolved to Z`
   - Strict glossary — no decisions, no implementation details

   **`DECISIONS.md`** — cross-feature architectural decisions:
   - Only decisions with impact beyond the originating feature
   - One feature is enough when the decision already constrains future work outside it; recurrence is not required
   - Each entry: decision summary, rationale, provenance (`[feature-name]`)
   - Feature-specific ADRs that have no cross-cutting impact are excluded

   **`ANTIPATTERNS.md`** — patterns that blocked or warned in review:
   - Only findings that appear in 2+ `REVIEW.md` files — single occurrences are noise
   - Grouped by category (test quality, architecture, scope creep, code style)
   - Each entry: pattern, severity (Blocking / Warning), provenance (`[feature-a, feature-b]`)

5. **Report.**
   ```
   Knowledge base updated: .neural/knowledge/
   — PROJECT-CONTEXT.md
   — GLOSSARY.md      (<N> terms)
   — DECISIONS.md     (<N> decisions)
   — ANTIPATTERNS.md  (<N> patterns)
   ```
