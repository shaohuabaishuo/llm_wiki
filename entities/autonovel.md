---
title: autonovel by Hermes Agent
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [agent, ai, cli, creative-writing, llm, pipeline]
sources: [raw/articles/autonovel-github-2026.md]
confidence: high
---

# autonovel (NousResearch/autonovel)

An autonomous novel writing pipeline built by Hermes Agent, consisting of 27 Python scripts that produce a complete novel from seed concept to PDF/ePub/audiobook/landing page (⭐727). Has produced a real 79,456-word novel.

## Overview
Created by NousResearch, this is a Python-based pipeline (not a reusable agent framework) that implements a modify-evaluate-keep/discard loop for fiction, inspired by karpathy/autoresearch. Each phase is a set of independent Python scripts that can be run separately.

## Pipeline Phases

### Phase 1: Foundation
Seed concept → world bible → character registry → outline with beats → foreshadowing ledger → canon cross-reference → voice fingerprint. Loop until foundation_score > 7.5.

### Phase 2: First Draft
Sequential chapter drafting with anti-pattern rules. Keep if score > 6.0, retry if not.

### Phase 3a: Automated Revision
Adversarial editing → cuts → reader panel (4 personas) → briefs → rewrite. Plateau detection stops the loop.

### Phase 3b: Opus Review Loop
Full manuscript to Claude Opus for dual-persona review (literary critic + professor of fiction).

### Phase 4: Export
LaTeX typesetting, art generation, audiobook scripts, ePub, landing page.

## Tools (27 scripts)
Key scripts: seed.py, gen_world.py, gen_characters.py, gen_outline.py, draft_chapter.py, run_drafts.py, evaluate.py, adversarial_edit.py, reader_panel.py, review.py, gen_brief.py, gen_revision.py, apply_cuts.py

## Quick Start
```bash
git clone <repo-url> && cd autonovel
cp .env.example .env
uv sync
uv run python seed.py
uv run python run_pipeline.py --from-scratch
```

## Notable
- Produced a complete 79,456-word English novel
- Not designed for Chinese novel creation
- Less active (last updated 2026-03-20)

## Related
- [[inkos-novel-agent]] — More mature and actively developed (⭐4,830)
- [[ai-novel-writing-assistant]] — Chinese-focused alternative with web UI
