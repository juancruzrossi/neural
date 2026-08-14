# Persistent Adversarial Review

Use the other supported coding agent as a read-only reviewer. Keep one reviewer
session per feature and reuse it for both passes. The reviewer produces
candidate findings; only the user may approve them, and only the primary agent
may apply them.

When Claude Code is primary, use Codex as reviewer. When Codex is primary, use
Claude Code. Launch the reviewer directly from the primary session; never ask
the user to open another terminal.

## Artifacts

Use only:

```text
.neural/wip/<feature>/
├── .adversarial-review.json
└── adversarial-review/
    ├── PLAN-REVIEW.md
    └── EXECUTION-REVIEW.md
```

The hidden JSON contains only `reviewer` (`claude` or `codex`) and
`session_id`. Never create temporary review Markdown or session metadata
Markdown. Create an artifact only after a command succeeds and its review is
non-empty. Preserve an existing artifact on failure.

## Prompts

Reference repository files instead of copying their contents. The plan prompt
must ask only for contradictions with `CONTEXT.md` or ADRs, ambiguous public
contracts, acceptance gaps, missing edge cases, non-observable behaviors, and
testing that cannot demonstrate behavior.

The execution prompt must tell Claude to invoke
`/neural:neural-review <feature> --adversarial`, or Codex to invoke
`$neural-review <feature> --adversarial`. Do not restate or imitate the skill.
Include the actual diff and changed-file set in the review input, then let the
skill inspect `CONTEXT.md`, `PLAN.md`, ADRs, `EXECUTION.md`, tests, surrounding
code, and repository instructions. The reviewer must distinguish freshly
verified evidence from claims it could not verify in read-only mode.

## Claude reviewer

Start the plan review without disabling persistence:

```bash
result=$(claude -p \
  --effort high \
  --allowedTools "Read,Grep,Glob,Skill" \
  --output-format json \
  "<plan-review-prompt>")
session_id=$(printf '%s' "$result" | jq -er \
  'select(.is_error == false) | .session_id | select(length > 0)')
review=$(printf '%s' "$result" | jq -er \
  'select(.is_error == false) | .result | select(length > 0)')
```

Resume the same session for execution review. Claude effort is not persistent,
so specify `high` again:

```bash
result=$(claude -p \
  --resume "$session_id" \
  --effort high \
  --allowedTools "Read,Grep,Glob,Skill" \
  --output-format json \
  "<execution-review-prompt>")
resumed_session_id=$(printf '%s' "$result" | jq -er \
  'select(.is_error == false) | .session_id | select(length > 0)')
test "$resumed_session_id" = "$session_id"
review=$(printf '%s' "$result" | jq -er \
  'select(.is_error == false) | .result | select(length > 0)')
```

## Codex reviewer

Start Codex in a read-only sandbox and capture JSONL:

```bash
result=$(codex exec -C "$PWD" --sandbox read-only --json \
  "<plan-review-prompt>")
session_id=$(printf '%s' "$result" | jq -ser \
  '[.[] | select(.type == "thread.started") | .thread_id] | last | select(length > 0)')
review=$(printf '%s' "$result" | jq -ser \
  '[.[] | select(.type == "item.completed" and .item.type == "agent_message") | .item.text] | last | select(length > 0)')
```

Resume with the verified Codex subcommand syntax:

```bash
result=$(codex exec -C "$PWD" --sandbox read-only --json \
  resume "$session_id" "<execution-review-prompt>")
resumed_session_id=$(printf '%s' "$result" | jq -ser \
  '[.[] | select(.type == "thread.started") | .thread_id] | last | select(length > 0)')
test "$resumed_session_id" = "$session_id"
review=$(printf '%s' "$result" | jq -ser \
  '[.[] | select(.type == "item.completed" and .item.type == "agent_message") | .item.text] | last | select(length > 0)')
```

Do not use `--ephemeral`: the execution pass must continue the plan-review
conversation.

## Persist and present

After the plan pass succeeds, create `adversarial-review/`, write the review to
`PLAN-REVIEW.md`, and write the reviewer and session ID to
`.adversarial-review.json`. Before the execution pass, require matching
metadata and resume that exact session. Write its successful result to
`EXECUTION-REVIEW.md`. Persist the reviewer text directly without a
provider-specific wrapper.

If valid metadata already exists, do not replace it or create another plan
review session. Surface the existing review and continue from its recorded
state.

Treat a missing CLI, missing skill, non-zero exit, malformed JSON, absent
session ID, empty result, provider mismatch, or failed resume as an unavailable
review, never as a clean review. Report the failure and recovery command; do
not silently start an independent replacement session. Show successful
findings with their provenance and ask the user which specific findings to
approve, defer, or reject. Apply nothing automatically.
