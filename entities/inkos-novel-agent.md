---
title: InkOS Novel Writing Agent
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [agent, ai, cli, creative-writing, llm]
sources: [raw/articles/inkos-github-2026.md]
confidence: high
---

# InkOS (Narcooo/inkos)

An autonomous novel writing AI Agent with native Chinese support (also supports English). The most popular (⭐4,830) and actively developed open-source novel writing agent as of April 2026.

## Overview
InkOS is a TypeScript npm package (`@actalk/inkos`) that orchestrates multiple specialized AI agents to write novels end-to-end. It covers xuanhuan (玄幻), xianxia (仙侠), urban (都市), sci-fi (科幻) and more genres. Licensed under AGPL-3.0.

## Quick Start
```bash
npm i -g @actalk/inkos
inkos config set-global --provider openai --api-key <key> --model <model>
inkos book create --title "My Novel" --genre xuanhuan
inkos write next "My Novel"
inkos export "My Novel" --format epub
```

## Key Features

### Multi-Agent Pipeline
7 specialized agents work in sequence per chapter: **Radar** (trends) → **Planner** (intent) → **Composer** (context) → **Architect** (structure) → **Writer** (text) → **Observer** (extraction) → **Reflector** (state update).

### 33-Dimension Audit
Continuity, character memory, item consistency, foreshadowing payoff, narrative pacing, emotional arcs, plus AI-trace detection to remove "LLM-flavored" writing.

### Style Imitation
Extracts statistical fingerprints from reference texts and injects them into target books. Supports command-line style analysis and import.

### Fan Fiction & Continuation
Import existing novels, auto-reverse-engineer 7 truth files, seamless continuation. Fanfic with 4 modes: canon, au, ooc, cp.

### Multi-Model Routing
Different agents can use different models. Writer on Claude (creative), Auditor on GPT-4o (fast/cheap), Radar on local models.

### Daemon Mode
Background auto-writing with human review gates. Notifications via Telegram, Feishu, WeCom, Webhook.

## Interfaces
- **CLI:** Primary interface, full command set
- **TUI:** Full-screen interactive dashboard (Ink + React)
- **Studio:** Local web workbench (InkOS Studio 2.0)
- **OpenClaw Skill:** Published for agent integration

## Related
- [[ai-novel-writing-assistant]] — Chinese full-stack alternative with web UI
- [[autonovel]] — Python-based pipeline by Hermes Agent
- [[kimi-writer]] — Simpler Python agent by Doriandarko
