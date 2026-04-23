---
source_url: https://github.com/Narcooo/inkos
ingested: 2026-04-23
sha256: 0f5a6b7c8d9e1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6
---

# InkOS - Autonomous Novel Writing AI Agent

## GitHub Metadata
- Full name: Narcooo/inkos
- Stars: 4,830
- Language: TypeScript (npm package: @actalk/inkos)
- License: AGPL-3.0
- Forks: 932
- Open Issues: 128
- Created: 2026-03-12
- Last updated: 2026-04-22 (yesterday at time of ingestion)
- Tags: agent, ai, ai-agent, ai-novel, ai-writing, chinese-novel, cli, cli-tool, creative-writing-ai, novel-ai, novel-generation, openclaw

## Description
Autonomous novel writing AI Agent — agents write, audit, and revise novels with human review gates. Covers xuanhuan (玄幻), xianxia (仙侠), urban (都市), sci-fi (科幻) and more genres. Supports sequel writing, spin-offs, fan fiction, and style imitation.

## Key Features

### Multi-Agent Pipeline
Each chapter is produced by a team of specialized agents:
- **Radar (雷达):** Scans platform trends and reader preferences
- **Planner (规划师):** Reads author intent + current focus + memory retrieval
- **Composer (编排师):** Selects relevant context from full truth files
- **Architect (建筑师):** Plans chapter structure, outline, scene beats, pacing
- **Writer (写手):** Generates body text with word count governance
- **Observer (观察者):** Extracts 9 categories of facts from text
- **Reflector (反射器):** Outputs JSON delta for world state updates

### Multi-Dimensional Audit (33 dimensions)
Continuity checks across character memory, item consistency, foreshadowing payoffs, outline deviation, narrative pacing, emotional arcs, etc. Built-in AI-trace detection identifies "LLM-flavored" expressions.

### Style Imitation
`inkos style analyze` extracts statistical fingerprints (sentence length distribution, word frequency, rhythm patterns) and LLM style guidelines from reference texts. `inkos style import` injects the style into target books.

### Fan Fiction
`inkos fanfic init --from source.txt --mode canon` with four modes: canon (canonical continuation), au (alternate universe), ooc (character reshaping), cp (couple-oriented).

### Multi-Model Routing
Different agents can use different models/providers. Writer can use Claude (creative), Auditor can use GPT-4o (fast/cheap), Radar can use local models (zero cost).

### Daemon Mode
`inkos up` starts background auto-writing. Non-critical pipeline issues auto-resolve; critical issues pause for human review. Notifications via Telegram, Feishu, WeCom, Webhook.

### Local Model Support
Any OpenAI-compatible API via `--provider custom`. Auto-degrades streaming when SSE not supported.

### Interoperability
Published as OpenClaw Skill. CLI + TUI Dashboard + Studio (local web workbench). `inkos interact --json --message "..."` for external agent integration.

## Quick Start
```bash
npm i -g @actalk/inkos
inkos config set-global --provider openai --api-key <key> --model <model>
inkos book create --title "My Novel" --genre xuanhuan
inkos write next "My Novel"
inkos export "My Novel" --format epub
```

## Pipeline Agents

| Agent | Role |
|-------|------|
| Radar | Platform trends and reader preferences |
| Planner | Read author intent + current focus + memory |
| Composer | Select relevant context from truth files |
| Architect | Plan chapter structure and pacing |
| Writer | Generate body text with word governance |
| Observer | Extract facts from written text |
| Reflector | Output JSON delta for runtime state |

## Studio 2.0
Web-based workbench: book management, chapter review/editing, real-time writing progress, market radar, data analysis, AI detection, style analysis, genre management, daemon control.

## TUI Dashboard
Full-screen interactive dashboard with conversational creation, natural language book operations, slash command autocomplete, theme animations, bilingual i18n.

## Reliability
- Auto snapshots per chapter, rollback any chapter
- Writer outputs pre-check table before drafting
- File locks prevent concurrent writes
- Foreshadowing system uses Zod schema validation
- `applyRuntimeStateDelta` for immutable updates + `validateRuntimeState` structural validation
