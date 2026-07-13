---
name: neural-archive
description: "Move completed features from wip to archive, then update the knowledge base"
---

# Neural Archive

Close a completed feature: move it from `.neural/wip/` to `.neural/archive/`, then harvest it into the knowledge base.

## Procedure

1. Resolve the feature from `$ARGUMENTS` or `.neural/wip/` — one directory: confirm "Archive `<name>`? (y/n)"; several: ask which; none: report "Nothing to archive" and stop.
2. If `.neural/archive/<name>/` already exists, stop and ask — never overwrite or nest.
3. Move it:
   ```bash
   mkdir -p .neural/archive/
   mv .neural/wip/<name>/ .neural/archive/<name>/
   ```
4. Report: `Feature '<name>' archived.`
5. Run neural-learn to harvest the newly archived feature into `.neural/knowledge/`.
