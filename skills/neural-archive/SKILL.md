---
name: neural-archive
description: "Archive a feature that passed neural-review by moving its folder to .neural/archive/. Use once REVIEW.md verdict is PASS."
---

# Neural Archive

Workflow: interview → plan → implement → review → archive. Previous: neural-review. Next: none.

Resolve the feature from the user's message or `.neural/wip/`; ask which one if ambiguous.

Require `.neural/wip/<feature>/REVIEW.md` with verdict PASS. Otherwise stop and point to the right previous step (neural-review for FAIL or missing).

Stop if `.neural/archive/<feature>/` already exists; never overwrite or nest an archive.

Ask once: `Archive <feature>? (y/n)`. On confirmation:

```bash
mkdir -p .neural/archive/
mv .neural/wip/<feature>/ .neural/archive/<feature>/
```

Git: if this is a git repository, commit the move locally. Never push.

Report: `Feature '<feature>' archived.`
