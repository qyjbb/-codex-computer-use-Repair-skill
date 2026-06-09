# Codex Computer Use Repair Skill

A standalone **Codex skill** for repairing **Codex Desktop Computer Use on Windows**.

This repository is for people who use Codex directly. Users do **not** need Hermes Agent to use this skill.

## What This Skill Does

This skill teaches Codex how to diagnose and repair Windows Computer Use problems in a safe order:

1. Check Codex Desktop installation and local environment.
2. Inspect and repair `~/.codex/config.toml`.
3. Enable `features.computer_use`.
4. Set `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE=1` when needed.
5. Rebuild or sync the `openai-bundled` marketplace mirror.
6. Restore `browser`, `chrome`, and `computer-use` plugin caches.
7. Repair Chrome native messaging host registration.
8. Work around Windows Store/MSIX protected file copy problems with byte-stream copy.
9. Treat MSIX/ASAR patching as a last resort only.

## Install for Codex

### Option 1: Download with curl

Run this in Git Bash, WSL, macOS/Linux shell, or any shell with `curl`:

```bash
mkdir -p ~/.codex/skills/codex-computer-use-repair
curl -L "https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md" \
  -o ~/.codex/skills/codex-computer-use-repair/SKILL.md
```

### Option 2: Download with PowerShell

Run this in PowerShell on Windows:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills\codex-computer-use-repair" | Out-Null
Invoke-WebRequest \
  -Uri "https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md" \
  -OutFile "$env:USERPROFILE\.codex\skills\codex-computer-use-repair\SKILL.md"
```

### Option 3: Manual install

1. Download `SKILL.md` from this repository.
2. Create this folder:
   ```text
   ~/.codex/skills/codex-computer-use-repair/
   ```
3. Put the file here:
   ```text
   ~/.codex/skills/codex-computer-use-repair/SKILL.md
   ```
4. Restart Codex so it reloads skills.

## Verify Installation

After installing, check that the file exists:

### Git Bash / WSL / macOS / Linux

```bash
ls ~/.codex/skills/codex-computer-use-repair/SKILL.md
```

### PowerShell

```powershell
Test-Path "$env:USERPROFILE\.codex\skills\codex-computer-use-repair\SKILL.md"
```

Then start a new Codex session and ask it to use the skill.

## Use in Codex

Open Codex and ask:

```text
Use the codex-computer-use-repair skill to inspect and repair my Windows Codex Desktop Computer Use setup.
Start with environment checks, config, plugin cache, and Chrome native messaging repair.
Do not do MSIX or ASAR patching unless I explicitly confirm.
```

Chinese prompt:

```text
请使用 codex-computer-use-repair skill 检查并修复我的 Windows Codex Desktop Computer Use。
先检查 Codex Desktop 安装状态、~/.codex/config.toml、CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE、openai-bundled marketplace、browser/chrome/computer-use 插件缓存，以及 Chrome native messaging host。
可以修复用户级配置、缓存和 Chrome host，但不要执行 MSIX/ASAR patch，除非先向我确认。
```

## When to Use

Use this skill if you are on Windows and:

- Codex Desktop does not show Computer Use.
- Computer Use is disabled even though your account/model should support it.
- Any App works but Chrome says the extension is not connected.
- The Chrome native messaging host is missing or broken.
- Plugin caches for `browser`, `chrome`, or `computer-use` are incomplete.
- Copying files from WindowsApps/MSIX package locations fails or hangs.

Do not use it for macOS/Linux issues, billing/account issues, or unrelated Chrome launch failures.

## Safety Notes

The skill is designed to avoid risky package patching unless necessary.

Lower-risk repair steps:

- Inspecting Codex Desktop state.
- Updating user-level Codex config.
- Setting user environment variables.
- Rebuilding plugin caches.
- Repairing Chrome native messaging manifests and HKCU registry entries.
- Copying runtime files into user-writable locations.

Higher-risk repair steps:

- Stopping Codex Desktop.
- Uninstalling the Store/MSIX package.
- Repacking or patching ASAR/MSIX files.
- Installing a locally signed package.

Do not allow high-risk steps unless you understand the tradeoff and have a rollback path.

## Update the Skill

Run the install command again to overwrite the local copy:

```bash
curl -L "https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md" \
  -o ~/.codex/skills/codex-computer-use-repair/SKILL.md
```

Then restart Codex.

## Uninstall

Remove the skill folder:

### Git Bash / WSL / macOS / Linux

```bash
rm -rf ~/.codex/skills/codex-computer-use-repair
```

### PowerShell

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\skills\codex-computer-use-repair"
```

## Files

- `SKILL.md` — Codex skill instructions.
- `README.md` — installation and usage instructions.

## License

MIT
