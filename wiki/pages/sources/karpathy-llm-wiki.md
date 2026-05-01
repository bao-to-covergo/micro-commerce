---
title: "Source: Karpathy — LLM Wiki"
type: source
tags: [meta, methodology, knowledge-management]
created: 2026-05-01
updated: 2026-05-01
url: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
raw: ../../raw/karpathy-llm-wiki.md
---

Andrej Karpathy's gist describing the "LLM Wiki" pattern: an LLM-maintained,
persistent, interlinked markdown knowledge base that sits between the human
and their raw sources. This wiki is a direct instantiation of that pattern.

## Key takeaways

- **Compounding artifact, not RAG.** The wiki is built and maintained
  incrementally. Synthesis happens once at ingest time and stays current,
  rather than being re-derived from raw documents on every query.
- **Three layers**: raw sources (immutable) → wiki (LLM-owned markdown) →
  schema (the operating manual, e.g. `AGENTS.md` / `CLAUDE.md`).
- **Three operations**: **ingest** new sources (touches 5–15 wiki pages),
  **query** the wiki (cite pages, optionally file synthesis back), **lint**
  for contradictions/orphans/stale claims/missing pages.
- **Two special files**: `index.md` (content-oriented catalog) and `log.md`
  (chronological, append-only, parseable header per entry).
- **Human role**: source curation, exploration, asking the right questions.
- **LLM role**: summarizing, cross-referencing, filing, bookkeeping —
  exactly the maintenance burden that causes humans to abandon wikis.
- **Storage**: just markdown in git. No database, no embeddings required at
  small/medium scale.
- **Spirit**: Vannevar Bush's Memex (1945), but with the LLM doing the
  associative-trail maintenance Bush couldn't solve.

## Why this page exists

This source justifies the **shape** of the wiki — `AGENTS.md`, `index.md`,
`log.md`, and the ingest/query/lint vocabulary all come from here. When the
human asks "why is the schema set up this way?", point them at this page.

## Pages derived from this source

- [[../../AGENTS.md]] — the wiki's operating manual
- [[../../README.md]] — human-facing overview

## Raw

- Captured locally: `wiki/raw/karpathy-llm-wiki.md` (verbatim copy).
- Original: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f.

Treat the local copy as authoritative for citation purposes — the gist URL
may change, but the captured file will not.
