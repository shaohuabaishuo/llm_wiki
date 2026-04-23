---
title: AI Novel Writing Assistant
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [agent, ai, framework, creative-writing, rag, orchestration, llm]
sources: [raw/articles/ai-novel-writing-assistant-github-2026.md]
confidence: high
---

# AI-Novel-Writing-Assistant (ExplosiveCoderflome)

A full-stack Chinese-focused open-source novel writing system (⭐725). Uses LangChain/LangGraph agent workflow with a web UI, targeting beginners who want AI-assisted long-form novel creation from a single idea.

## Overview
Unlike CLI-focused tools, this is a complete web application (React + Express + Prisma) with a visual interface. It uses LangChain/LangGraph for agent orchestration and Qdrant for RAG-based knowledge retrieval. Has a Windows desktop version available via Releases.

## Architecture
- **Frontend:** React + Vite + Plate editor
- **Backend:** Express + Prisma (SQLite default, Qdrant for RAG)
- **AI:** LangChain + LangGraph
- **Desktop:** Windows Setup.exe + portable versions
- **Project:** pnpm workspace monorepo

## Key Features

### Auto-Director Mode
Takes a single concept and generates complete novel direction candidates. Supports iterative refinement and three progression modes (milestone review, auto-advance, auto-execute first 10 chapters).

### Creative Hub
Unified creation center where dialogue, planning, tool calls, execution status, checkpoint resume all converge.

### Writing Style Engine
Styles are saved as reusable assets. Extract features from existing text, enable/disable individually in a visible feature pool.

### Worldbuilding System
Structured world assets with layers, snapshots, deepening QA, consistency checks.

### Dynamic Character System
Relationship stages, volume-level responsibilities, absence risk analysis, candidate new characters.

### Knowledge Base / RAG
Document management with vector + keyword search, feeding back into planning and generation.

### Multi-Provider Model Routing
OpenAI, DeepSeek, SiliconFlow, xAI support. Different pipeline stages can use different providers.

## Project Philosophy
- Targets absolute beginners
- Priority on "finish the whole book" over "write beautifully"
- AI as a system role (planning, orchestrating, executing, tracking) — not just text completion

## Related
- [[inkos-novel-agent]] — More mature CLI-based alternative (⭐4,830)
- [[autonovel]] — Python pipeline by Hermes Agent
