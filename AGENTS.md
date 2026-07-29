# Neural SDD — Project Rules

## What is this
A Spec-Driven Development plugin for AI coding agents. It ships as a plugin for both Claude Code and Codex from the same repo. Skills live in `skills/`. Manifests:
- Claude Code: `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json`
- Codex: `.codex-plugin/plugin.json` + `.agents/plugins/marketplace.json`

## Versioning (do this on EVERY release)
Each platform has one authoritative plugin manifest. On every release, bump the same semver version in:
1. `.codex-plugin/plugin.json` — Codex manifest version
2. `.claude-plugin/plugin.json`

Do not duplicate plugin versions in marketplace catalogs. Claude resolves the version from `.claude-plugin/plugin.json`; Codex resolves it from `.codex-plugin/plugin.json`.

## When adding or removing skills
1. Create/delete the skill directory in `skills/`
2. Update `README.md` — add/remove the skill section
3. Bump the version in both plugin manifests (see Versioning above)

## Conventions
- Skill logic lives in `SKILL.md` — one per skill. Auxiliary reference files (e.g., format templates) may sit alongside `SKILL.md` and be linked from it, so they load on-demand and keep `SKILL.md` light at trigger time.
- Skill instructions in English, concise, imperative
- Skill folders use `neural-` prefixed names (`neural-interview`, `neural-plan`, etc.) so skills stay namespaced on every platform
- Claude Code invokes skills as `/neural:<name>` (e.g., `/neural:neural-plan`)
- Codex invokes skills with `$<name>` (e.g., `$neural-plan`) or implicit matching
- Code examples in skills and references use Python. Use Bash only for shell commands and `md` or `text` for artifact templates.
- Descriptions: just `"<what it does>"` — no boilerplate prefixes or suffixes
- Commits: conventional commits in English
- PRs: squash & merge, delete branch after
