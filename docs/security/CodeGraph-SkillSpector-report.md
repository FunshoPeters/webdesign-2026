# CodeGraph security scan: NVIDIA SkillSpector

- **Target:** colbymchenry/codegraph, commit f72c9d2 (1 Nov 2026), version 1.6.2 (same as npm `latest`)
- **Scanner:** NVIDIA SkillSpector, static analysis only (`--no-llm`; no LLM API key in this workspace)
- **Date:** 13 Nov 2026

## Verdict from SkillSpector

| Part scanned | Risk score | Recommendation | Coverage |
|---|---|---|---|
| Installers (install.sh, install.ps1, package.json, agent instruction files) | 100 / CRITICAL | DO_NOT_INSTALL | 40% |
| Telemetry + self-upgrade (`src/telemetry`, `src/upgrade`, telemetry-worker) | 100 / CRITICAL | DO_NOT_INSTALL | 64% |
| CLI + file watcher (`src/bin`, `src/sync`) | 83 / CRITICAL | DO_NOT_INSTALL | 45% |
| Agent wiring (`src/installer`, `src/mcp`) | 100 / CRITICAL | DO_NOT_INSTALL | 54% |

A full-repo scan didn't finish in 20 minutes, so the code that matters for safety was scanned in these four parts. Coverage figures are what SkillSpector reported as fully inspected.

## What the HIGH findings actually are (manual review)

| Finding | Where | Real risk? |
|---|---|---|
| Downloads and runs a script (`curl … \| sh`) | install.sh, `codegraph upgrade` | **Real supply-chain exposure**: whatever is on GitHub at that moment runs on your machine. Avoidable: install a pinned version from npm and never run `codegraph upgrade`. |
| Self-modification | `codegraph upgrade` | Same as above: the tool can replace itself. Avoid the upgrade command. |
| Reads and writes agent config (`~/.claude/settings.json`, `~/.codex/config.toml`, `~/.gemini/…`) | installer | **Expected**: this is how it connects to Claude Code and the other tools. But it also adds, by default, a hook that runs on **every prompt** you send in Claude Code, and `codegraph upgrade` adds that hook to your global Claude settings automatically. |
| `rm -f` / `rm -rf` | install.sh uninstall branch, install.ps1 warning text | Benign: only removes its own install folder when you ask it to uninstall. |
| `curl` POSTs to an external server | telemetry-worker/scripts | Benign for you: these are the developer's server-side test scripts and aren't shipped in the package. |
| `--noCheck` | package.json build script | False positive (TypeScript compiler flag). |

MEDIUM findings: anonymous telemetry on by default (can be switched off); unpinned `npx` references in docs; the CLI's own "skip confirmation" flags; a `sudo` hint in an error message. None of these showed malicious intent.

**No evidence was found of code that sends your source code or secrets anywhere.** The npm package (1.6.2) has no install scripts.

## Bottom line

SkillSpector's formal recommendation is **DO NOT INSTALL**, mainly because of the remote-script installer, self-update, and agent-config writing. On manual review, these are normal behaviours for a tool of this kind, not signs of malware. But they are real trust points.

If you do use it, the safer setup is:
1. Install from npm, pinned: `npm install -g @colbymchenry/codegraph@1.6.2`
2. Don't use the `curl | sh` installer, and never run `codegraph upgrade`.
3. Run `codegraph telemetry off`.
4. Connect it to this project only (`.mcp.json` in the repo), not globally, and decline the every-prompt hook.

**Limitations:** this was a static scan with partial coverage and no LLM semantic pass. The prebuilt platform binaries that npm installs (`@colbymchenry/codegraph-<platform>`) were not scanned.
