# Adversarial Plan Review

Use the other installed agent in read-only mode. Give it file references to the
feature `CONTEXT.md`, `PLAN.md`, ADR directory, and relevant project files; do
not inline their contents.

Ask for:

- critical conflicts with the approved context or ADRs;
- missing acceptance coverage, edge cases, or public-contract decisions;
- behaviors coupled to implementation rather than a public interface;
- citations to behavior IDs and specification sections;
- `unverified` whenever evidence is unavailable.

Keep the review concise and save it to a temporary output file. A non-zero exit
or empty output is an unavailable review, not a clean review. Show the full
feedback and ask whether to apply all, cherry-pick, or ignore it.

Suggested invocations:

```bash
# Claude Code → Codex
codex exec --ephemeral -C "$PWD" -s read-only -o <output-file> -

# Codex → Claude Code
claude --print --no-session-persistence \
  --allowedTools "Read,Grep,Glob" > <output-file>
```

Allow enough time for the other agent to boot. Apply nothing until the user
approves specific feedback.
