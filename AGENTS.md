# Working with Neural

1. `neural-interview` clarifies a feature request into `.neural/wip/<feature>/CONTEXT.md`. Done when nothing that changes scope or acceptance is still open and the user confirms.
2. `neural-plan` turns `CONTEXT.md` into `.neural/wip/<feature>/PLAN.md`: behaviors, public interfaces, vertical slices. Requires the user's explicit approval (`Status: approved`) before moving on.
3. `neural-implement` requires an approved `PLAN.md` and builds it one vertical slice at a time, test first through the public interface.
4. `neural-review` checks the implementation against `PLAN.md` on two axes — spec fidelity and repo standards — and writes `.neural/wip/<feature>/REVIEW.md` with a PASS/FAIL verdict. FAIL sends the feature back to `neural-implement`.
5. `neural-archive` moves a PASSed feature from `.neural/wip/<feature>/` to `.neural/archive/<feature>/`.

Git: if the project is a git repository, each skill commits locally as it finishes meaningful work. Never push.

## Releasing

Bump the same semver version in `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` on every release. Adding or removing a skill also updates `README.md`.
