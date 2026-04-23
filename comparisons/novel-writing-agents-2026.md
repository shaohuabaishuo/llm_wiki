---
title: Open-Source Novel Writing Agent Comparison
created: 2026-04-23
updated: 2026-04-23
type: comparison
tags: [comparison, agent, ai, creative-writing, llm]
sources:
  - raw/articles/inkos-github-2026.md
  - raw/articles/ai-novel-writing-assistant-github-2026.md
  - raw/articles/autonovel-github-2026.md
confidence: high
---

# Open-Source Novel Writing Agent Comparison

A comprehensive comparison of open-source AI agents for novel writing, surveyed April 2026.

## At a Glance

| Feature | InkOS | AI-Novel-Assistant | autonovel | kimi-writer | gemini-writer |
|---------|-------|-------------------|-----------|-------------|---------------|
| **Stars** | **⭐4,830** | ⭐725 | ⭐727 | ⭐564 | ⭐276 |
| **Language** | TypeScript | TypeScript (full-stack) | Python | Python | Python |
| **Install** | `npm i -g @actalk/inkos` | Clone + monorepo setup | `git clone + uv sync` | `git clone` | `git clone` |
| **Chinese Support** | **Native** (also EN) | **Native (中文优先)** | English only | English | English |
| **Web UI** | Studio 2.0 | Creative Hub | None | None | None |
| **TUI** | Yes (Ink dashboard) | None | None | None | None |
| **CLI** | Yes (primary) | No (Web only) | Yes (script-based) | Yes | Yes |
| **Multi-Agent** | **7 agents** | LangChain/LangGraph | Script pipeline | Single agent | Single agent |
| **Audit System** | **33 dimensions** | Quality control loop | Score-based eval | None | None |
| **Style Imitation** | Yes | Yes (写法引擎) | Voice fingerprint | No | No |
| **RAG / Knowledge** | Truth files system | **Qdrant vector DB** | Canon cross-ref | No | No |
| **Fanfic Mode** | Yes (4 modes) | No | No | No | No |
| **Daemon/Background** | Yes (inkos up) | No | No | No | No |
| **Export Formats** | EPUB, plain text | Not specified | **PDF, ePub, audiobook** | Plain text | Plain text |
| **Model Routing** | **Multi-model** | Multi-provider | Claude Opus only | Kimi-only | Gemini-only |
| **Windows Desktop** | No | **Yes (Setup.exe)** | No | No | No |
| **Recent Update** | **2026-04-22** | 2026-04-23 | 2026-03-20 | 2026-01-29 | 2025-12-24 |
| **License** | AGPL-3.0 | Not specified | None | Other | Other |

## Detailed Comparison

### Maturity & Community
**InkOS** dominates with 4,830 stars — more than 6x the next competitor. It has 932 forks, 128 open issues (sign of active development), and updated daily. ^[raw/articles/inkos-github-2026.md] The AI-Novel-Assistant and autonovel are close at ~725 stars each but are much younger projects. ^[raw/articles/ai-novel-writing-assistant-github-2026.md] ^[raw/articles/autonovel-github-2026.md]

### Chinese Language Support
For Chinese novel writing, **InkOS** is the clear leader with native Chinese interface, Chinese documentation, support for Chinese genres (玄幻, 仙侠, 都市), and all commands working in Chinese. ^[raw/articles/inkos-github-2026.md] The **AI-Novel-Assistant** is also Chinese-first but has fewer features and a smaller community. ^[raw/articles/ai-novel-writing-assistant-github-2026.md] autonovel, kimi-writer, and gemini-writer are English-only. ^[raw/articles/novel-writing-agent-search-2026.md]

### Ease of Setup
**InkOS** wins with `npm i -g @actalk/inkos` — one command install. ^[raw/articles/inkos-github-2026.md] The **AI-Novel-Assistant** requires cloning a monorepo and setting up frontend + backend. ^[raw/articles/ai-novel-writing-assistant-github-2026.md] **autonovel** needs Python + uv. ^[raw/articles/autonovel-github-2026.md] kimi-writer and gemini-writer are simple clones but less capable. ^[raw/articles/novel-writing-agent-search-2026.md]

### Feature Completeness
**InkOS** has the most comprehensive feature set: 7-agent pipeline, 33-dimension audit, style imitation, fanfic, daemon mode, multi-model routing, TUI, Studio, export to EPUB. ^[raw/articles/inkos-github-2026.md] The **AI-Novel-Assistant** is strong on worldbuilding, character system, and RAG but lacks CLI, daemon mode, and TUI. ^[raw/articles/ai-novel-writing-assistant-github-2026.md] **autonovel** has a complete evaluation + revision loop but lacks Chinese support and ongoing development. ^[raw/articles/autonovel-github-2026.md]

## Recommendations

### For Chinese Novel Writers (User Goal)
**InkOS** is the #1 recommendation:
- Most mature and actively developed
- Native Chinese support (also English)
- One-command install, immediate use
- Most comprehensive multi-agent pipeline
- TUI and Studio for visual operation
- Best export support (EPUB for phone/Kindle reading)
- Strongest community (4,830 stars)

### For Beginner-Friendly Web UI
**AI-Novel-Writing-Assistant** is worth considering if the user wants a full web interface with visual worldbuilding and style engine management, and doesn't mind the heavier setup.

### For Technical / English-Only Users
**autonovel** is interesting for Python users who want a complete English-only pipeline and don't need ongoing active development. It has produced a real 79K-word novel.

### Skip
**kimi-writer** and **gemini-writer** are too simple and inactive to recommend for serious novel writing.

## Verdict
For the user's goal of Chinese novel creation, **InkOS** is the best choice by a wide margin: most stars, most active, most features, easiest setup, best Chinese support.

## Related
- [[inkos-novel-agent]] — Detailed entity page
- [[ai-novel-writing-assistant]] — Full-stack alternative
- [[autonovel]] — Python pipeline
- [[best-novel-writing-agent-2026]] — Research query with ranking
