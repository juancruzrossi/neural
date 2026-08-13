# Review Seal

Use SHA-256 over file bytes. Write `REVIEW.md` as UTF-8 with LF line endings.

## Reviewed files

Record each reviewed file with the lowercase SHA-256 of its content bytes.

## Feature tree

Reject symlinks and newline characters in paths. Include every regular file
under the feature directory except `REVIEW.md`.

1. Express each path relative to the feature directory with POSIX separators.
2. Sort paths by their UTF-8 byte sequence.
3. Emit `<lowercase file digest>  <path>\n` in UTF-8 for each path.
4. Record the lowercase SHA-256 of the complete emitted byte sequence as
   `Feature-Tree-SHA256`.

## Review file

Complete the report except for its seal. Compute SHA-256 after removing the
entire `Review-SHA256` line, including its LF. Record the lowercase digest on
that line. Verification must remove the line again and hash the remaining raw
bytes.
