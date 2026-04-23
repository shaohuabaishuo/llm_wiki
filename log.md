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