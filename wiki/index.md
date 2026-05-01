# Wiki Index

Catalog of every page in the wiki. The LLM updates this on every ingest and
rebuilds it during a lint pass. Read this first when answering a query.

Categories: **Entities** (concrete things), **Concepts** (ideas/patterns),
**Sources** (one summary per item in `raw/`), **Analyses** (synthesis pages
filed back from queries).

## Entities

- [MicroCommerce Platform](pages/entities/microcommerce-platform.md) — the showcase e-commerce system this wiki is currently scoped to
- [ApiService](pages/entities/api-service.md) — modular-monolith ASP.NET Core backend hosting all feature slices
- [YARP Gateway](pages/entities/yarp-gateway.md) — reverse proxy fronting ApiService (CORS, rate limiting, auth, request IDs)

## Concepts

- [Schema-per-Feature](pages/concepts/schema-per-feature.md) — single Postgres database `appdb`, one schema per vertical slice
- [CQRS with MediatR](pages/concepts/cqrs-mediatr.md) — commands/queries split, validation + result-checking pipeline behaviors
- [Checkout Saga](pages/concepts/checkout-saga.md) — MassTransit state machine orchestrating reservation → payment → confirm

## Sources

- [MicroCommerce CLAUDE.md](pages/sources/microcommerce-claude-md.md) — project conventions, tech stack, architecture overview (root `CLAUDE.md`)
- [Karpathy LLM Wiki gist](pages/sources/karpathy-llm-wiki.md) — the original pattern this wiki implements

## Analyses

_(none yet — file synthesis here when queries produce keepers)_
