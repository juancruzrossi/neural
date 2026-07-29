---
name: neural-archive
description: "Archive a freshly verified feature and refresh project knowledge"
---

# Neural Archive

Resolve the feature from `$ARGUMENTS` or `.neural/wip/`; ask which one when
ambiguous.

Require `REVIEW.md` with verdict `PASS` or `PASS WITH WARNINGS`. Missing or
`FAIL` reviews are not archivable; point to neural-review. Warnings require
explicit acceptance.

Verify `## Reviewed state` before asking to move:

- recompute every recorded product-file hash;
- in git repos, compare the recorded `HEAD` and changed-file set;
- any drift stops the archive and requires a fresh review;
- for a legacy review without recorded state, explain that freshness is
  unverifiable and require explicit risk acceptance.

Stop if `.neural/archive/<feature>/` already exists. Never overwrite or nest an
archive.

Show the verdict and freshness result, then ask once:
`Archive <feature>? (y/n)`. On confirmation:

```bash
mkdir -p .neural/archive/
mv .neural/wip/<feature>/ .neural/archive/<feature>/
```

Report `Feature '<feature>' archived.`

Then load [neural-learn](../neural-learn/SKILL.md) and follow it to refresh the
knowledge base. Do not look for a shell command named `neural-learn`.

Leave the archive and knowledge changes local. Never stage, commit, or push;
preserve pre-existing staged and unrelated changes as found.
