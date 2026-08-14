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

Read [REVIEW-SEAL.md](../neural-review/references/REVIEW-SEAL.md) before
verifying freshness.

Verify `## Reviewed state` before asking to move:

- recompute every recorded file SHA-256 and require an exact match;
- recompute `Feature-Tree-SHA256` from the complete feature file set and
  require an exact match;
- recompute `Review-SHA256` after removing its entire line and require an exact
  match; never refresh a seal during archive;
- in git repos, compare the recorded `HEAD` and status;
- any drift stops the archive and requires a fresh review;
- for a legacy review missing reviewed state or any required seal field,
  explain that freshness is unverifiable and require explicit risk acceptance.

Before asking, run the neural-learn source-coverage preflight against every
existing archive and the candidate feature in place. A missing required source
stops the archive.

Stop if `.neural/archive/<feature>/` already exists. Never overwrite or nest an
archive.

Show the verdict and freshness result, then ask once:
`Archive <feature>? (y/n)`. On confirmation:

1. Repeat the complete freshness and source-coverage preflight. Confirm the
   archive destination still does not exist. Stop on drift or collision.
2. Move the feature:

```bash
mkdir -p .neural/archive/
mv .neural/wip/<feature>/ .neural/archive/<feature>/
```

Report `Feature '<feature>' archived.`

Then load [neural-learn](../neural-learn/SKILL.md) and follow it to refresh the
knowledge base. Do not look for a shell command named `neural-learn`.

Leave the archive and knowledge changes local. Never stage, commit, or push;
preserve pre-existing staged and unrelated changes as found.
