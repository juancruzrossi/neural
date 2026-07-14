---
name: neural-archive
description: "Move completed features from wip to archive, then update the knowledge base"
---

# Neural Archive

## Procedure

1. Resolve the feature from `$ARGUMENTS` or `.neural/wip/` — one directory: confirm "Archive `<name>`? (y/n)"; several: ask which; none: report "Nothing to archive" and stop.
2. If `REVIEW.md` is missing or its verdict is FAIL, say so — archive only with explicit confirmation.
3. If `.neural/archive/<name>/` already exists, stop and ask — never overwrite or nest.
4. Move it:
   ```bash
   mkdir -p .neural/archive/
   mv .neural/wip/<name>/ .neural/archive/<name>/
   ```
5. Report: `Feature '<name>' archived.`
6. Run neural-learn.
