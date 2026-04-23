---
source_url: https://github.com/ExplosiveCoderflome/AI-Novel-Writing-Assistant
ingested: 2026-04-23
sha256: 1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b
---

# AI-Novel-Writing-Assistant - 面向长篇小说的 AI 生产系统

## GitHub Metadata
- Full name: ExplosiveCoderflome/AI-Novel-Writing-Assistant
- Stars: 725
- Language: TypeScript (Monorepo: React + Vite frontend, Express + Prisma backend)
- License: Not specified
- Forks: Not available
- Created: Not available (recent, likely early 2026)
- Last updated: 2026-04-23 (very active)
- Description: 面向长篇小说创作的 AI Native 开源系统，用 Agent、世界观、写法引擎、RAG 和整本生产工作流，帮助新手从一句灵感走到完整小说。

## Architecture
- Full-stack web application
- Frontend: React + Vite (Plate editor)
- Backend: Express + Prisma
- AI: LangChain / LangGraph
- Vector DB: Qdrant (for RAG)
- Desktop: Windows Setup.exe and portable versions available via GitHub Releases

## Key Features

### 1. AI Auto-Director (自动导演)
Creates complete novel direction from a single concept. Generates multiple book directions, title groups, and allows iterative refinement. Supports three progression modes: review per milestone, auto-advance to writable, auto-execute first 10 chapters.

### 2. Creative Hub
Unified creation center: dialogue, planning, tool calls, execution status, checkpoint resume, and state summaries all in one interface.

### 3. Chapter Execution Pipeline
Single-chapter runtime, chapter-by-chapter execution, and batch whole-book pipeline on the same main chain. Structured planning, chapter table of contents, asset preparation.

### 4. Writing Style Engine (写法引擎)
Styles are saved, edited, bound, tested, and reused as long-term assets. Extract style features from existing text, with visible feature pools that can be enabled/disabled individually.

### 5. Worldbuilding System (世界观)
Structured world assets: layers, snapshots, deepening QA, consistency checks, novel binding. Not just static setting text but dynamic, queryable world knowledge.

### 6. Character System (角色体系)
Dynamic character assets: relationship stages, volume-level responsibilities, absence risks, candidate new characters fed into subsequent planning and generation.

### 7. Knowledge Base / RAG
Document management, vector search, keyword search, rebuild task tracking. Knowledge feeds back into continuation writing, planning, and content generation.

### 8. Model Routing
Supports OpenAI, DeepSeek, SiliconFlow, xAI, and more. Planning, writing, and review can use different model providers.

### 9. Quality Control
Post-chapter state feedback loop that affects subsequent chapters, volume-level rhythm, and replanning when needed.

## Project Philosophy
- Target users: beginners with no writing experience, not experienced authors
- Priority: "finish the whole book" first, then optimize "how well it's written"
- AI is not just text completion but a system role involved in planning, judging, orchestrating, executing, and tracking

## Typical Usage Flow
1. Enter a concept on the novel creation page → Auto-director generates direction candidates
2. Project settings: genre, selling points, target reader experience, first 30 chapters commitment
3. Story macro-planning, character preparation, worldbuilding
4. Volume strategy / volume skeleton → chapter breakdown
5. Bind knowledge base docs and writing style assets
6. Chapter execution: write, audit, repair
7. Batch production tasks for acceleration
