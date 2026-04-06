# Installing Neural for Codex

Neural installs two things: **prompts** (commands the user invokes) and **skills** (the logic behind each command).

## Installation

```bash
git clone https://github.com/juancruzrossi/neural.git ~/.codex/neural

# Install skills
mkdir -p ~/.agents/skills
ln -s ~/.codex/neural/skills ~/.agents/skills/neural

# Install prompts (commands)
mkdir -p ~/.codex/commands
for f in ~/.codex/neural/prompts/*.md; do
  ln -sf "$f" ~/.codex/commands/neural-"$(basename "$f")"
done
```

If symlinks already exist, the `-sf` flag overwrites them.

### Windows

```powershell
git clone https://github.com/juancruzrossi/neural.git "$env:USERPROFILE\.codex\neural"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\neural" "$env:USERPROFILE\.codex\neural\skills"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\commands"
Get-ChildItem "$env:USERPROFILE\.codex\neural\prompts\*.md" | ForEach-Object {
  Copy-Item $_.FullName "$env:USERPROFILE\.codex\commands\neural-$($_.Name)"
}
```

## Verify

```bash
ls ~/.agents/skills/neural/*/SKILL.md
ls ~/.codex/commands/neural-*.md
```

You should see 10 skills and 9 prompts.

## Update

```bash
cd ~/.codex/neural && git pull
```

Symlinks point to the updated files automatically.

## Uninstall

```bash
rm -f ~/.codex/commands/neural-*.md
rm -f ~/.agents/skills/neural
rm -rf ~/.codex/neural
```
