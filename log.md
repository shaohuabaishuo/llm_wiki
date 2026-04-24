# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-04-23] create | Hermes Tool Ecosystem inventory
- Created [concepts/hermes-tool-ecosystem.md](concepts/hermes-tool-ecosystem.md) – categorized tool inventory with official documentation links (17 categories, 50+ tools)
- Updated [index.md](index.md) with new page entry
- Domain: AI/ML tools, frameworks, and agents
- Structure created with SCHEMA.md, index.md, log.md
- Git repository: https://github.com/shaohuabaishuo/llm_wiki.git

## [2026-04-23] ingest | Hermes Agent installation guide
- Source: https://hermes-agent.nousresearch.com/docs/getting-started/installation
- Raw saved: raw/articles/hermes-installation.html, raw/articles/hermes-installation.md
- Entity created: entities/hermes-agent-installation.md

## [2026-04-23] ingest | LLM Wiki skill documentation
- Source: hermes://skill/llm-wiki
- Raw saved: raw/articles/llm-wiki-skill-2026.md
- Concept created: concepts/llm-wiki.md
- Schema updated: added "wiki" tag to taxonomy

## [2026-04-23] ingest | Open-Source Novel Writing Agent Survey
- Ingested 3 raw source files:
  - raw/articles/inkos-github-2026.md
  - raw/articles/ai-novel-writing-assistant-github-2026.md
  - raw/articles/autonovel-github-2026.md
- Created 5 entity pages:
  - entities/inkos-novel-agent.md
  - entities/ai-novel-writing-assistant.md
  - entities/autonovel.md
  - entities/kimi-writer.md
  - entities/gemini-writer.md
- Created 1 comparison page:
  - comparisons/novel-writing-agents-2026.md
- Updated index.md (total pages: 2 → 8)

## [2026-04-23] update | Wiki quality pass + new pages
- Added `creative-writing` and `pipeline` tags to SCHEMA.md taxonomy
- Created raw source: raw/articles/novel-writing-agent-search-2026.md
- Fixed source references on kimi-writer and gemini-writer entity pages
- Added provenance markers to comparisons/novel-writing-agents-2026.md
- Created concept page: concepts/ai-novel-writing.md
- Created query page: queries/best-novel-writing-agent-2026.md
- Updated index.md (total pages: 8 → 10)

## [2026-04-23] lint | 4 issues found (3 pre-existing + 1 fixed)
- Ran lint on all 10 wiki pages
- 3 pre-existing broken links in entities/hermes-agent-installation.md (out of scope)
- Fixed orphan: added [[best-novel-writing-agent-2026]] link from comparisons page
- All 10 pages now have inbound links; no new orphans or broken links

## [2026-04-23] create | Obsidian 入门使用说明书
|- Added `obsidian` tag to SCHEMA.md taxonomy (under Tools)
|- Created concept page: concepts/obsidian-guide.md
|- Updated index.md (total pages: 11 → 12)

## [2026-04-23] create | Hermes 编程最佳实践
|- Created concept page: concepts/hermes-programming-best-practices.md — comprehensive Chinese guide for using Hermes Agent for programming (14 sections covering workflow, tools, TDD, debugging, subagents, code review, git, cron, pitfalls)
|- Updated index.md (total pages: 12 → 13)

## [2026-04-24] create | Inkos 连接本地 Ollama 模型完整配置指南
- Added `ollama` and `wsl` tags to SCHEMA.md taxonomy
- Created concept page: concepts/inkos-ollama-local-setup.md — 从零开始将 Inkos 接入本地 Ollama 模型的完整操作流程，含 WSL2 网络穿透、.env 和 inkos.json 双配置、模型切换、故障排查清单（6 步操作流程 + 4 常见问题 + 7 项检查清单）
- Cross-references: [[inkos-novel-agent]], [[ai-novel-writing]], [[best-novel-writing-agent-2026]]
- Source: 两次真实调试会话经验（WSL2 网络隔离诊断 → Ollama 0.0.0.0 绑定 → Inkos 双文件配置）
- Updated index.md (total pages: 13 → 14)

## [2026-04-24] create | Hermes 接入本地 DeepSeek 模型配置指南
- Created concept page: concepts/hermes-local-deepseek-setup.md — Hermes Agent 对接本地 Ollama DeepSeek 模型的完整操作指南（10 章节：环境概览、WSL2 网络配置、Ollama 上下文优化、config.yaml 配置、模型切换、工具调用限制分析、验证流程、常见问题、检查清单）
- 关键技术点：Ollama 默认 context 不足且需手动设置 OLLAMA_CONTEXT_LENGTH、deepseek-r1:8b 在 Ollama 上不支持工具调用、WSL2 mirrored 网络模式 vs NAT 模式、named custom provider 实现云端/本地无缝切换
- Cross-references: [[hermes-agent-installation]], [[hermes-programming-best-practices]], [[hermes-tool-ecosystem]], [[inkos-ollama-local-setup]], [[llm-wiki]]
- 配置产物：~/.hermes/config.yaml 添加了 custom_providers.local-deepseek + .wslconfig 配置 mirrored 网络
- Updated index.md (total pages: 14 → 15)