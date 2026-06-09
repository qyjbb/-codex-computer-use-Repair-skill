---
name: codex-computer-use-repair
description: Use when Codex Desktop on Windows cannot enable Computer Use, Any App works but Chrome is disconnected, or plugin caches/configuration for browser, chrome, and computer-use are missing or corrupted.
version: 1.0.0
author: qyjbb
license: MIT
platforms: [windows]
metadata:
  hermes:
    tags: [codex, computer-use, windows, chrome, desktop, repair]
    related_skills: [codex, systematic-debugging]
---

# Codex Computer Use Repair

## Installation

Install this skill directly into Codex by placing this file at:

```text
~/.codex/skills/codex-computer-use-repair/SKILL.md
```

Quick install with `curl`:

```bash
mkdir -p ~/.codex/skills/codex-computer-use-repair
curl -L "https://raw.githubusercontent.com/qyjbb/-codex-computer-use-Repair-skill/main/SKILL.md" \
  -o ~/.codex/skills/codex-computer-use-repair/SKILL.md
```

Restart Codex after installing so it can reload skills.

## Overview

This skill repairs Codex Desktop Computer Use on Windows by working from the lowest-risk fixes toward the highest-risk package patching path. Start by checking the local environment, then restore Codex plugin caches and configuration, repair Chrome native messaging, and only consider MSIX/ASAR patching after all normal local repair paths are exhausted.

The core principle is: **repair user-level configuration and runtime assets first; patch installed packages only as a last resort.**

## When to Use

Use this skill when:

- Codex Desktop on Windows shows Computer Use as unavailable or disabled.
- Any App works, but Chrome says the extension or connection is not ready.
- `browser`, `chrome`, or `computer-use` plugin caches are missing, incomplete, or stale.
- `~/.codex/config.toml` lacks the Computer Use feature flag.
- Windows Store/MSIX file protection causes normal copy tools to hang or fail.
- A previous repair attempt changed Codex Desktop files but Computer Use still does not appear.

Do not use this skill when:

- The user is on macOS, Linux, or WSL without Windows Codex Desktop.
- The issue is only model authentication, billing, or account access.
- Chrome itself cannot launch independently.
- The user has not consented to package-level patching or uninstall/reinstall operations.

## Safety Model

Apply fixes in this order:

1. **Inspect only** — identify Codex Desktop, config, env vars, plugin caches, and Chrome integration state.
2. **User-level repair** — restore marketplace mirror/cache, update `~/.codex/config.toml`, and set environment variables.
3. **Chrome host repair** — rebuild the native messaging manifest and HKCU registry entry.
4. **Executable relocation** — copy `codex.exe`, `node.exe`, and `node_repl.exe` to a user-writable runnable location if Chrome native messaging cannot execute them from the protected package path.
5. **MSIX/ASAR patching** — last resort only; this may stop Codex Desktop, uninstall a Store package, or install a locally signed package.

Never start with MSIX/ASAR patching. It is the riskiest step and should only follow a complete local repair attempt.

## Environment Checks

Before changing anything, inspect these areas:

- **Codex Desktop install state:** confirm the app is installed and locate its package/runtime files.
- **Codex config:** inspect `~/.codex/config.toml` for feature flags and sandbox settings.
- **Windows feature env var:** check `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE`.
- **Marketplace mirror:** inspect the `openai-bundled` marketplace mirror.
- **Plugin cache:** confirm `browser`, `chrome`, and `computer-use` plugin caches exist and contain expected files.
- **Chrome integration:** inspect native messaging manifest, registry entry, and Chrome profile/extension state.

Record the current state before writing changes so the user can understand what was repaired.

## Local Computer Use Repair

If Computer Use plugins or config are missing or wrong:

1. Sync or rebuild the `openai-bundled` marketplace mirror.
2. Restore the `browser`, `chrome`, and `computer-use` plugin cache directories.
3. Ensure `~/.codex/config.toml` contains:
   ```toml
   [features]
   computer_use = true
   ```
4. Ensure the Windows sandbox mode is set to `unelevated` when the config supports it.
5. Set the user environment variable:
   ```text
   CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE=1
   ```
6. Restart Codex Desktop and re-check whether Computer Use appears.

Prefer idempotent writes. If a config key already exists, update it instead of appending duplicates.

## WindowsApps Protected File Workaround

Many failures come from Windows Store/MSIX package files that have Application Protected or encrypted attributes. These attributes can cause tools such as `robocopy` or `Copy-Item` to hang, fail, or preserve unusable metadata.

When copying files out of protected package locations:

- Avoid metadata-preserving copy paths.
- Use byte-stream content copy instead of normal shell copy operations.
- Write to a user-writable directory before using the file from Chrome native messaging or other child processes.
- Verify the copied file can be executed or read from the target location.

The goal is to copy file bytes, not Windows package attributes.

## Chrome Control Chain Repair

If Any App works but Chrome reports that the extension is not connected, repair the native messaging chain:

1. Rebuild the native messaging manifest for `com.openai.codexextension`.
2. Ensure the manifest points to a runnable host path in a user-writable location when the package path is protected.
3. Write or update this HKCU registry key:
   ```text
   HKCU\Software\Google\Chrome\NativeMessagingHosts\com.openai.codexextension
   ```
4. Ensure the registry default value points to the native messaging manifest path.
5. Copy required runtime executables, when needed, to a user-writable runnable directory:
   - `codex.exe`
   - `node.exe`
   - `node_repl.exe`
6. Check whether Chrome is using the expected profile and extension installation.
7. Restart Chrome and Codex Desktop before retesting.

A Chrome-side failure is often not a Computer Use model problem. Treat it as a native messaging host registration and executable-access problem first.

## Last-Resort MSIX/ASAR Patch

Only consider MSIX/ASAR patching after all of these are true:

- Computer Use is enabled in config.
- The marketplace mirror and plugin caches are complete.
- `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE=1` is set.
- Chrome native messaging manifest and HKCU registry entry are correct.
- Required host/runtime executables are accessible from user-writable paths.
- Codex Desktop and Chrome have both been restarted and retested.

Warn the user before this stage because it may require:

- Stopping Codex Desktop.
- Uninstalling the Windows Store/MSIX package.
- Repacking or patching ASAR/MSIX contents.
- Installing a locally signed package.
- Reverting if the patched package fails to launch.

Do not perform package-level patching without explicit user confirmation.

## Verification Checklist

After repair, verify:

- [ ] `~/.codex/config.toml` enables `features.computer_use`.
- [ ] `CODEX_ELECTRON_ENABLE_WINDOWS_COMPUTER_USE` is set for the user environment.
- [ ] `browser`, `chrome`, and `computer-use` plugin caches exist.
- [ ] The `openai-bundled` marketplace mirror is present and current enough for the installed Codex Desktop version.
- [ ] The Chrome native messaging manifest exists.
- [ ] `HKCU\Software\Google\Chrome\NativeMessagingHosts\com.openai.codexextension` points to that manifest.
- [ ] Native messaging host executables live in a path Chrome can execute.
- [ ] Chrome uses the profile where the Codex extension is installed.
- [ ] Codex Desktop shows Computer Use as available after restart.
- [ ] Chrome Computer Use connects successfully, not only Any App.

## Common Pitfalls

1. **Starting with MSIX/ASAR patching.** This is risky and often unnecessary. Repair config, caches, and Chrome native messaging first.

2. **Using normal copy tools on protected package files.** Windows Store package attributes can break `robocopy`, `Copy-Item`, or metadata-preserving copies. Use byte-stream copy for protected files.

3. **Only fixing Any App.** Any App can work while Chrome still fails because Chrome depends on the native messaging manifest, registry key, extension profile, and executable access.

4. **Appending duplicate config keys.** Duplicate TOML keys can make the config invalid or ambiguous. Update existing keys in place.

5. **Forgetting restarts.** Environment variables, Chrome native messaging hosts, and Codex Desktop feature flags may not refresh until Chrome and Codex Desktop are restarted.

6. **Assuming account/model problems.** If the UI shows Chrome extension disconnected, inspect the local native messaging chain before debugging provider authentication.
