# Wiki Log

Append-only chronological record of every operation against the wiki. New
entries go at the bottom. Each entry header follows
`## [YYYY-MM-DD] <op> | <subject>` so recent activity is greppable:

```
grep "^## \[" wiki/log.md | tail -20
```

---

## [2026-05-01] init | wiki scaffold

- created: `AGENTS.md`, `README.md`, `index.md`, `log.md`
- created: `pages/entities/`, `pages/concepts/`, `pages/sources/`, `raw/`
- seeded entity pages: `microcommerce-platform`, `api-service`, `yarp-gateway`
- seeded concept pages: `schema-per-feature`, `cqrs-mediatr`, `checkout-saga`
- seeded source pages: `microcommerce-claude-md`, `karpathy-llm-wiki`
- notes: scoped to MicroCommerce codebase; first real sources are the repo's own docs

## [2026-05-01] ingest | FluxCD documentation mirror

- fetched: 296 pages from <https://fluxcd.io/flux/> (deep crawl from `/flux/get-started/`)
- saved to: `raw/fluxcd/` mirroring the URL path layout
- index: `raw/fluxcd/INDEX.md` lists every page grouped by section
- sections covered: get-started, concepts, installation (bootstrap + configuration), guides, integrations (aws/gcp/azure/cross-cloud), monitoring, use-cases, components (source/kustomize/helm/notification/image), cmd (full CLI reference), cheatsheets, security, releases, faq, flux-gh-action, flux-e2e, gitops-toolkit, migration
- notes: raw HTML→markdown via html2text; source URL preserved in YAML frontmatter; pages immutable per `AGENTS.md`. No `pages/` entries created yet — drop a follow-up "ingest fluxcd" to file source/concept pages
