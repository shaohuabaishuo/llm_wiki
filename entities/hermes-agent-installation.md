---
title: Hermes Agent Installation
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [tools, installation, cli, agent]
sources: [raw/articles/hermes-installation.md]
confidence: high
---
# Hermes Agent Installation

Hermes Agent is a CLI AI assistant that can be installed via a one-line installer.

## Quick Install

### Linux / macOS / WSL2
```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

### Android / Termux
Same command; the installer detects Termux automatically and switches to a tested Android flow.

## What the Installer Does
The installer handles everything automatically:
- Dependencies: Python, Node.js, ripgrep, ffmpeg
- Repository clone
- Virtual environment setup
- Global `hermes` command installation
- LLM provider configuration

## After Installation
Reload your shell and start chatting:
```bash
source ~/.bashrc   # or source ~/.zshrc
hermes             # Start chatting!
```

## Prerequisites
Only Git is required. The installer automatically installs:
- uv (fast Python package manager)
- Python 3.11
- Node.js v22
- ripgrep
- ffmpeg

## Troubleshooting
| Problem | Solution |
|---------|----------|
| `hermes: command not found` | Reload your shell (`source ~/.bashrc`) or check PATH |
| `API key not set` | Run `hermes model` to configure provider |
| Missing config after update | Run `hermes config check` then `hermes config migrate` |

For more diagnostics, run `hermes doctor`.

## Related
- [[Hermes Agent]] (the main entity)
- [[CLI AI assistants]] (broader category)
- [[Termux setup]] (Android installation)

## References
^[raw/articles/hermes-installation.md]