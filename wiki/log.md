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
