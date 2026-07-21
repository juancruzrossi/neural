---
name: neural-archive
description: "Move completed features from wip to archive, then update the knowledge base"
---

# Neural Archive

## Procedure

1. Resolve the feature from `$ARGUMENTS` or `.neural/wip/` — one directory: confirm "Archive `<name>`? (y/n)"; several: ask which; none: report "Nothing to archive" and stop.
2. If `REVIEW.md` is missing or its verdict is FAIL, say so — archive only with explicit confirmation.
3. If `REVIEW.md` contains `## Reviewed state`, recompute its product-file hashes. If `HEAD`, changed-file set, or any hash differs, stop: "Reviewed state has drifted; run neural-review again." Never archive a stale verdict. If no reviewed state is recorded, warn that freshness cannot be verified and require explicit confirmation.
4. If `.neural/archive/<name>/` already exists, stop and ask — never overwrite or nest.
5. Move it:
   ```bash
   mkdir -p .neural/archive/
   mv .neural/wip/<name>/ .neural/archive/<name>/
   ```
6. Report: `Feature '<name>' archived.`
7. Run neural-learn.
