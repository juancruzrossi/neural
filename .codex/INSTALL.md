# Installing Neural for Codex

Neural installs as **skills only** — no prompts, no commands. The `commands/` directory in this repo is for Claude Code and must be ignored.

## Prerequisites

- Git
- Codex CLI installed

## Installation

```bash
git clone https://github.com/juancruzrossi/neural.git ~/.codex/neural
mkdir -p ~/.agents/skills
ln -s ~/.codex/neural/skills ~/.agents/skills/neural
```

If `~/.agents/skills/neural` already exists, remove it first.

Restart Codex.

### Windows

```powershell
git clone https://github.com/juancruzrossi/neural.git "$env:USERPROFILE\.codex\neural"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\neural" "$env:USERPROFILE\.codex\neural\skills"
```

## Verify

```bash
ls -la ~/.agents/skills/neural
find ~/.agents/skills/neural -maxdepth 2 -name SKILL.md | sort
```

You should see `~/.agents/skills/neural` pointing to `~/.codex/neural/skills` with 10 SKILL.md files.

## Usage

Reference skills by name in your request:

- `Use neural-interview for this feature`
- `Use neural-plan for the active feature`
- `Use neural-review before we close this task`

## Available Skills

| Skill | Purpose |
|-------|---------|
| `using-neural` | Session guidance, task suggestion, resume help |
| `neural-interview` | Clarify requirements → `BRIEF.md` |
| `neural-plan` | Explore codebase, generate → `PLAN.md` |
| `neural-execute` | Execute plan in dependency waves |
| `neural-review` | Verify implementation against brief and plan |
| `neural-address-review` | Fix issues found during review |
| `neural-quick` | Small-task fast path (no `.neural/` artifacts) |
| `neural-debug` | Root-cause debugging |
| `neural-status` | Show active feature progress |
| `neural-archive` | Move completed work from `wip/` to `archive/` |

## Update

```bash
cd ~/.codex/neural && git pull
```

## Uninstall

```bash
rm ~/.agents/skills/neural
rm -rf ~/.codex/neural
```
