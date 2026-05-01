# Wiki

An LLM-maintained knowledge base built on Andrej Karpathy's
[LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

The idea: instead of treating documents as a RAG store the LLM re-reads on
every question, the LLM **incrementally compiles** what it learns into a
persistent, interlinked set of markdown pages. Synthesis happens once at
ingest time and stays current. Queries read pre-compiled pages, not raw
documents. The wiki is a compounding artifact.

## What's where

| Path | Purpose |
|---|---|
| `AGENTS.md` | Schema — the operating manual every LLM agent must follow |
| `index.md` | Content catalog — every page listed by category |
| `log.md` | Chronological, append-only record of ingests/queries/lints |
| `raw/` | Immutable source documents (LLM reads, never writes) |
| `pages/entities/` | One page per concrete thing (service, library, person) |
| `pages/concepts/` | One page per idea or pattern |
| `pages/sources/` | One summary page per item under `raw/` |

## How to use it

The three operations, all driven from chat:

- **Ingest** — `"Ingest wiki/raw/<file>"` (or paste a URL). The LLM clips
  the source if needed, summarizes, files a `pages/sources/<slug>.md`,
  updates affected entity/concept pages, refreshes `index.md`, and appends
  to `log.md`.
- **Query** — Ask a question. The LLM consults `index.md` first, then drills
  into the relevant pages. If the answer is non-trivial it offers to file
  it back as a permanent analysis page.
- **Lint** — `"Lint the wiki"` (do this every ~10 ingests). The LLM finds
  contradictions, orphans, stale claims, missing pages, broken links, and
  index drift, then proposes fixes.

The full conventions live in [`AGENTS.md`](./AGENTS.md). Read that before
asking the LLM to do anything substantive.

## Current scope

Seeded as a knowledge base **about the MicroCommerce codebase itself** —
the project's architecture, patterns, libraries, and decisions. The first
real source is the repo's own `CLAUDE.md`. To repurpose it for a different
domain, edit the *Scope* section of `AGENTS.md` and replace the seed
pages — the operating manual stays the same.

## Editing rules for humans

- **Don't edit `pages/` directly.** Tell the agent what's wrong instead;
  the schema invariants (cross-references, index, log) only stay consistent
  when one party owns the writes.
- **Do drop sources into `raw/`.** That's how you grow the wiki.
- **Do fix `AGENTS.md`** when a convention isn't working. The schema is
  meant to co-evolve.

## Tooling tips

- The wiki is plain markdown in git, so version history, branching, and
  collaboration come for free.
- It also opens cleanly in **Obsidian** — point a vault at `wiki/` and you
  get the graph view, backlinks, and the Web Clipper for free.
- At small scale `index.md` + `grep` is the search layer. Reach for
  [qmd](https://github.com/tobi/qmd) (BM25/vector hybrid + MCP) only when
  the index file gets unwieldy.

## See also

- [`pages/sources/karpathy-llm-wiki.md`](./pages/sources/karpathy-llm-wiki.md)
  — summary of the source pattern this wiki implements
- [`raw/karpathy-llm-wiki.md`](./raw/karpathy-llm-wiki.md) — verbatim copy
  of the original gist
