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

## [2026-05-01] ingest | Dapr docs (pubsub, workflow, jobs/scheduler, secrets)

- scope change: repointed wiki to Dapr docs (subset). MicroCommerce seed
  pages retained under "Legacy" in `index.md` per don't-delete-without-asking.
- updated: `AGENTS.md` *Scope* section now describes Dapr focus and the four
  building blocks in scope (pub/sub, workflow, jobs/scheduler, secrets).
- created concept pages:
  - `pages/concepts/dapr-pubsub.md`
  - `pages/concepts/dapr-workflow.md`
  - `pages/concepts/dapr-jobs.md`
  - `pages/concepts/dapr-secrets.md`
- created entity page: `pages/entities/dapr-scheduler-service.md`
- created source pages (31):
  - pubsub (12): overview, howto, cloudevents, subscription-methods,
    deadletter, ttl, bulk, scopes, raw, routing, namespace, statefulset
  - workflow (10): overview, architecture, features, patterns, concurrency,
    versioning, retention, multi-app, howto-author, howto-manage
  - jobs/scheduler (6): jobs-overview, jobs-features, jobs-howto,
    scheduler-service, scheduler-kubernetes-persistence,
    scheduler-self-hosted-persistence
  - secrets (3): overview, howto, scopes
- rebuilt `index.md` with Dapr sections; legacy MicroCommerce entries moved
  to a "Legacy" footer.
- raw sources read end-to-end: 31 files under `raw/developing-applications/
  building-blocks/{pubsub,workflow,jobs,secrets}/`, `raw/concepts/dapr-services/
  scheduler.md`, `raw/operations/hosting/{kubernetes,self-hosted}/
  *-persisting-scheduler.md`. Other building blocks (state, bindings, service
  invocation, actors, configuration, distributed lock, conversation,
  cryptography) and the wider sdkdocs/getting-started/contributing/reference
  trees remain in `raw/` but are out of scope until requested.
- notes: cross-references between the four concept pages established;
  `[[dapr-runtime]]` and `[[dapr-actors]]` referenced but pages not yet
  created (treat as deferred — file when/if those building blocks are
  ingested).
