---
title: MicroCommerce Platform
type: entity
tags: [platform, overview]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

MicroCommerce is a showcase e-commerce platform that demonstrates a modern .NET
modular-monolith intentionally designed for gradual extraction into microservices.
A Next.js storefront talks through a YARP gateway to a single ASP.NET Core
backend whose features are organized as vertical slices.

## Topology

```
Next.js (MicroCommerce.Web)
    │
    ▼
YARP Gateway (MicroCommerce.Gateway)  ◀── CORS, rate limit, auth, X-Request-ID
    │
    ▼
ApiService (MicroCommerce.ApiService)
    │
    ▼
PostgreSQL (appdb, schema-per-feature)
```

Aspire (`MicroCommerce.AppHost`) is the entry point for local dev and wires
PostgreSQL → ApiService → Gateway → Frontend together. The frontend resolves
the gateway URL via the Aspire-injected `services__gateway__https__0` env var.

## Feature slices

Each lives under `src/MicroCommerce.ApiService/Features/<Name>/` with its own
`Domain/Application/Infrastructure` folders and an owned `DbContext`:

- **Catalog** — products, categories, images
- **Cart** — guest + authenticated shopping cart
- **Ordering** — checkout, orders, the [[checkout-saga]]
- **Inventory** — stock management
- **Profiles** — user profiles, addresses, avatars
- **Reviews** — verified-purchase product reviews
- **Wishlists** — authenticated user wishlists
- **Messaging** — dead letter queue UI

## Cross-references

- [[api-service]] — the backend hosting all slices
- [[yarp-gateway]] — the reverse proxy in front of it
- [[schema-per-feature]] — the persistence pattern
- [[cqrs-mediatr]] — the command/query pattern inside each slice
- [[checkout-saga]] — the cross-slice workflow

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md) — entire page derived from this
