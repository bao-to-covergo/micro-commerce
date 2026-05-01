# LLM Wiki — Schema and Operating Manual

This file is the schema for the LLM Wiki living under `wiki/`. It tells any LLM agent
(Claude Code, Codex, etc.) how the wiki is organized and exactly what to do when the
human asks to **ingest**, **query**, or **lint**. The pattern is from Andrej Karpathy's
[LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

The human curates sources and asks questions. The LLM does all the writing,
cross-referencing, and bookkeeping. **Humans rarely edit wiki pages directly** —
nudge the agent instead so the schema-driven invariants stay intact.

## Directory layout

```
wiki/
  AGENTS.md            # this file — the schema
  README.md            # human-facing overview
  index.md             # content catalog (LLM-maintained, every page listed)
  log.md               # chronological append-only record of ingests/queries/lints
  raw/                 # immutable source documents (PDFs, .md clippings, images)
  pages/
    entities/          # one page per concrete thing (service, person, product, library)
    concepts/          # one page per idea/pattern (CQRS, saga, schema-per-feature)
    sources/           # one summary page per item under raw/
```

The wiki is just markdown in git. No database, no embeddings — `index.md` plus
`grep` is the search layer until size forces something heavier.

## Page conventions

Every page starts with YAML frontmatter so the index and any future Dataview-style
tooling can parse it:

```yaml
---
title: Schema-per-Feature
type: concept            # entity | concept | source | analysis
tags: [persistence, ddd, postgres]
created: 2026-05-01
updated: 2026-05-01
sources: [sources/microcommerce-claude-md.md]
---
```

Body sections, in order:

1. **One-paragraph summary** — what this page is about, written so the reader can
   stop after the first paragraph and still know the answer.
2. **Body** — the actual content. Use H2/H3. Keep it tight.
3. **Cross-references** — bulleted list of `[[wiki links]]` to related pages.
4. **Sources** — bulleted list of `sources/*.md` pages this content draws from,
   with the specific claim each source backs up where non-obvious.

Filenames are `kebab-case.md`. Page titles are Title Case. Internal links use
relative paths (`../concepts/cqrs.md`) so they survive being moved into Obsidian
or rendered on GitHub.

## The three operations

### 1. Ingest

Trigger: human drops a file into `raw/` (or pastes a URL) and says "ingest this".

Steps:

1. Read the source end to end. If it's a URL, fetch it and save a markdown
   clipping into `raw/` first so the source is captured immutably.
2. Discuss the key takeaways with the human in chat — **before** writing.
   Confirm scope: what's worth filing, what's noise.
3. Create `pages/sources/<slug>.md` with frontmatter, a one-paragraph summary,
   the takeaways, and verbatim quotes for any claim likely to be cited later.
4. Touch every wiki page the source affects:
   - update or create relevant `entities/` and `concepts/` pages
   - add cross-references both directions (if A links to B, B links to A)
   - flag contradictions explicitly: `> ⚠️ Contradicts [[other-page]] (2026-04-12): …`
5. Update `index.md` — every new page gets listed under the right category.
6. Append one line to `log.md` using the format below.

A single ingest typically touches 5–15 pages. That's normal.

### 2. Query

Trigger: human asks a question.

Steps:

1. Read `index.md` first. Pick the 3–8 pages most likely to contain the answer.
2. Read those pages. If they cite source pages, follow the citations only when
   the wiki page is ambiguous or thin.
3. Answer with citations: every non-trivial claim links to the wiki page (and
   transitively the source) it came from.
4. **If the answer is novel or non-trivial, offer to file it back** as
   `pages/concepts/<slug>.md` or `pages/analysis/<slug>.md`. Synthesis should
   compound, not vanish into chat history.

Answer format follows the question — a paragraph for simple questions, a table
for comparisons, a markdown page for anything worth keeping.

### 3. Lint

Trigger: human says "lint the wiki" (do this every ~10 ingests).

Check for:

- **Contradictions** — pages making opposing claims without a `⚠️ Contradicts` note.
- **Stale claims** — superseded by a newer source but not yet revised.
- **Orphans** — pages with zero inbound links (probably mis-categorized).
- **Missing pages** — concepts mentioned ≥3 times across the wiki but lacking
  their own page.
- **Broken links** — internal `[[...]]` or relative paths that don't resolve.
- **Index drift** — pages on disk not in `index.md`, or vice versa.

Output a punch list. Fix what's safe (typos, broken links, index drift).
For substantive issues (contradictions, missing pages), propose changes and
wait for the human.

## index.md format

Grouped by category. Each line: `- [Title](relative/path.md) — one-line summary`.
Categories in order: Entities, Concepts, Sources, Analyses. Append-only when
ingesting; rewrite during lint.

## log.md format

Append-only, one entry per operation. Each entry starts with a parseable header:

```
## [YYYY-MM-DD] <op> | <subject>

- pages touched: ...
- notes: ...
```

`<op>` is one of `ingest`, `query`, `lint`, `analysis`. Consistent prefixes
mean `grep "^## \[" wiki/log.md | tail -20` always works as a recent-activity feed.

## Scope of this particular wiki

This wiki is seeded as a **knowledge base about the MicroCommerce codebase
itself** — architecture, patterns, decisions, libraries. The first sources are
the project's own docs (`CLAUDE.md`, `README.md`, `docs/`). It can be repointed
to any domain by replacing the seed pages and editing this section; the
operating manual above stays unchanged.

## What the LLM must NOT do

- Don't edit anything under `raw/`. Sources are immutable.
- Don't delete pages without asking. Stale pages get marked, not removed.
- Don't fabricate citations. Every source link must resolve to a real
  `pages/sources/*.md` file backed by something in `raw/`.
- Don't skip `log.md`. The log is the audit trail.
- Don't expand scope silently. If an ingest touches a new domain not yet in
  the wiki, surface that to the human first.
