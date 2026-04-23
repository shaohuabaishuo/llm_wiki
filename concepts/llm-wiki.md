---
title: LLM Wiki
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [wiki, knowledge-base, research, notes, markdown]
sources: [raw/articles/llm-wiki-skill-2026.md]
confidence: high
---
# LLM Wiki

A **LLM Wiki** is a persistent, compounding knowledge base built as interlinked markdown files, following the pattern popularized by [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Unlike traditional Retrieval-Augmented Generation (RAG) which rediscovers knowledge from scratch per query, the wiki compiles knowledge once and keeps it current through systematic ingestion, cross-referencing, and maintenance.

## Core Philosophy

**Division of labor:** The human curates sources and directs analysis. The agent (e.g., [[Hermes Agent Installation]]) summarizes, cross-references, files, and maintains consistency.

**Compounding knowledge:** Each new source enriches existing pages, creating a network of interconnected facts that becomes more valuable over time. Contradictions are flagged, synthesis reflects everything ingested, and cross-references are already in place.

## Architecture: Three Layers

### Layer 1 – Raw Sources (`raw/`)
Immutable source material stored in structured subdirectories:
- `articles/` – Web articles, clippings
- `papers/` – PDFs, arXiv papers
- `transcripts/` – Meeting notes, interviews
- `assets/` – Images, diagrams referenced by sources

Each raw file includes frontmatter with `source_url`, `ingested` date, and `sha256` hash to detect content drift.

### Layer 2 – The Wiki (`entities/`, `concepts/`, `comparisons/`, `queries/`)
Agent-owned markdown files created and maintained through ingestion:
- **Entities** – Notable people, organizations, products, models
- **Concepts** – Topics, techniques, methodologies
- **Comparisons** – Side-by-side analyses
- **Queries** – Filed query results worth preserving

Every wiki page includes YAML frontmatter with metadata and must link to at least two other pages via `[[wikilinks]]`.

### Layer 3 – The Schema (`SCHEMA.md`)
Defines the wiki's domain, conventions, tag taxonomy, page thresholds, and update policies. Ensures consistency across all agent operations.

## Core Operations

### 1. Ingest
Integrating new sources into the wiki:
1. Capture raw source with proper frontmatter
2. Discuss takeaways with the user
3. Check existing pages to avoid duplicates
4. Create/update wiki pages following page thresholds
5. Update navigation (`index.md`, `log.md`)
6. Report changes

A single source can trigger updates across 5–15 wiki pages—this compounding effect is intentional.

### 2. Query
Answering questions using compiled knowledge:
1. Consult `index.md` and search for relevant pages
2. Read relevant pages
3. Synthesize answer, citing sources with `[[wikilinks]]`
4. File valuable answers as `queries/` or `comparisons/` pages
5. Log the query

### 3. Lint
Health-checking the wiki for:
- Orphan pages (no inbound links)
- Broken wikilinks
- Index completeness
- Frontmatter validation
- Stale content (>90 days)
- Contradictions
- Source drift
- Page size (>200 lines)
- Tag audit
- Log rotation (>500 entries)

## Key Differences from RAG

| Aspect | Traditional RAG | LLM Wiki |
|--------|----------------|----------|
| **Knowledge storage** | Vector embeddings in database | Interlinked markdown files |
| **Query cost** | Re‑retrieval + generation per query | Pre‑compiled answers, minimal lookup |
| **Cross‑references** | None (each query is isolated) | Explicit `[[wikilinks]]` between pages |
| **Contradiction handling** | Returns conflicting snippets | Flagged in frontmatter, surfaced during lint |
| **Maintenance** | Re‑embedding entire corpus | Incremental updates, drift detection |
| **Human‑in‑the‑loop** | Optional | Essential for curation and direction |

## Best Practices

- **Always orient first** – Read `SCHEMA.md`, `index.md`, and recent `log.md` before any operation in a new session.
- **Follow page thresholds** – Create pages only when an entity/concept appears in 2+ sources or is central to one source.
- **Maintain cross‑references** – Every page must link to at least two other pages; check back‑links.
- **Use taxonomy tags** – Only tags defined in `SCHEMA.md`; add new tags there first.
- **Mark confidence** – Set `confidence: medium` or `low` for opinion‑heavy, fast‑moving, or single‑source claims.
- **Handle contradictions explicitly** – Note both positions with dates/sources, mark `contested: true`, flag for review.
- **Keep pages scannable** – Split pages exceeding ~200 lines; move deep analysis to dedicated pages.
- **Rotate the log** – When `log.md` exceeds 500 entries, rename to `log‑YYYY.md` and start fresh.

## Integration with Tools

- **Obsidian** – The wiki directory works as an Obsidian vault out of the box, with `[[wikilinks]]`, Graph View, and Dataview queries.
- **Git** – Version control enables collaboration, backup, and multi‑device synchronization.
- **Hermes Agent** – Uses the `llm‑wiki` skill to perform ingestion, querying, and linting operations.
- **Obsidian Headless** – Enables continuous sync on headless servers while browsing the vault on desktop/mobile.

## Related Concepts

- [[Hermes Agent Installation]] – CLI agent that implements the LLM Wiki pattern via the `llm‑wiki` skill.
- Knowledge management – Broader field encompassing wikis, note‑taking systems, and personal knowledge bases.
- Agent‑assisted research – Paradigm where AI agents help humans curate and synthesize information.

## Open Questions / Debates

- **Optimal page size** – Should pages be kept under 200 lines for scannability, or is deeper integration within a single page more valuable?
- **Automation vs. curation** – How much of the ingestion and cross‑referencing can be automated without losing human judgment?
- **Scalability** – At what point does a wiki become too large for effective navigation, and what structural changes are needed (topic maps, sub‑wikis)?
- **Multi‑user collaboration** – How to handle concurrent edits, conflict resolution, and permission models in a Git‑based wiki.

## References

- Karpathy, Andrej. *LLM Wiki pattern*. 2024. [GitHub Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- Hermes Agent. *llm‑wiki skill documentation*. 2026. [hermes://skill/llm‑wiki](hermes://skill/llm‑wiki)