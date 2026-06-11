# Neural SDD — Project Rules

## What is this
A Spec-Driven Development plugin for Claude Code. Codex installs the same root skills globally via `.codex/install.sh`. Skills live in `skills/`.

## When adding or removing skills
1. Create/delete the skill directory in `skills/`
2. Update `README.md` — add/remove the skill section
3. Propose a version bump in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`

## Conventions
- Skill logic lives in `SKILL.md` — one per skill. Auxiliary reference files (e.g., format templates) may sit alongside `SKILL.md` and be linked from it, so they load on-demand and keep `SKILL.md` light at trigger time.
- Skill instructions in English, concise, imperative
- Skill folders use `neural-` prefixed names (`neural-interview`, `neural-plan`, etc.) so skills stay namespaced on every platform
- Claude Code invokes skills as `/neural:<name>` (e.g., `/neural:neural-plan`)
- Codex invokes skills with `$<name>` (e.g., `$neural-plan`) or implicit matching
- Descriptions: just `"<what it does>"` — no boilerplate prefixes or suffixes
- Commits: conventional commits in English
- PRs: squash & merge, delete branch after
