# Codex Computer Use Repair Skill

A Hermes Agent skill for repairing **Codex Desktop Computer Use on Windows**.

This skill helps diagnose and repair cases where Codex Desktop cannot enable Computer Use, the `browser` / `chrome` / `computer-use` plugin cache is missing, Any App works but Chrome is disconnected, or Windows Store/MSIX protected files block normal repair steps.

## What This Skill Does

The skill follows a low-risk to high-risk repair order:

1. Checks the local Codex Desktop environment.
2. Repairs `~/.codex/config.toml` and enables `features.computer_use`.
3. Sets `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE=1` when needed.
4. Rebuilds or syncs the `openai-bundled` marketplace mirror and plugin caches.
5. Repairs the Chrome native messaging host chain.
6. Uses byte-stream copy when Windows protected package attributes break normal copy tools.
7. Treats MSIX/ASAR patching as a last resort only.

## Who Should Use It

Use this skill if you are on **Windows** and:

- Codex Desktop does not show Computer Use.
- Computer Use is disabled even though your account/model should support it.
- Any App works but Chrome says the extension is not connected.
- The Chrome native messaging host is missing or broken.
- Plugin caches for `browser`, `chrome`, or `computer-use` are incomplete.
- Copying files from WindowsApps/MSIX package locations fails or hangs.

Do not use it for macOS/Linux issues, account billing issues, or general Chrome launch failures unrelated to Codex Desktop.

## Install in Hermes Agent

Install directly from this repository:

```bash
hermes skills install https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md
```

Then start a new Hermes session or reload skills if your interface supports it.

## Use the Skill

In Hermes CLI, explicitly load the skill:

```bash
hermes -s codex-computer-use-repair
```

Or ask Hermes naturally:

```text
Use the codex-computer-use-repair skill to check and repair Codex Desktop Computer Use on Windows.
```

If you are already inside a Hermes chat session, you can try:

```text
/skill codex-computer-use-repair
```

Then ask:

```text
检查并修复我的 Codex Desktop Windows Computer Use，先只做低风险本地修复，不要做 MSIX/ASAR patch，除非我确认。
```

## Recommended Prompt

Use this prompt for a safe first pass:

```text
请使用 codex-computer-use-repair skill 检查我的 Windows Codex Desktop Computer Use 环境。
先检查 Codex Desktop 安装状态、~/.codex/config.toml、CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE、openai-bundled marketplace、browser/chrome/computer-use 插件缓存，以及 Chrome native messaging host。
可以修复用户级配置、缓存和 Chrome host，但不要执行 MSIX/ASAR patch，除非先向我确认。
```

## Safety Notes

This skill is designed to avoid risky package patching unless necessary.

Safe or lower-risk actions include:

- Inspecting Codex Desktop state.
- Updating user-level Codex config.
- Setting user environment variables.
- Rebuilding plugin caches.
- Repairing Chrome native messaging manifests and HKCU registry entries.
- Copying runtime files into user-writable locations.

Higher-risk actions include:

- Stopping Codex Desktop.
- Uninstalling the Store/MSIX package.
- Repacking or patching ASAR/MSIX files.
- Installing a locally signed package.

Do not allow the high-risk steps unless you understand the tradeoff and have a rollback path.

## Verify Installation

After installing, check that Hermes can see the skill:

```bash
hermes skills list
```

You should see:

```text
codex-computer-use-repair
```

You can also inspect the installed skill:

```bash
hermes skills inspect https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md
```

## Files

- `SKILL.md` — the Hermes skill definition.
- `README.md` — public usage instructions for this repository.

## License

MIT
