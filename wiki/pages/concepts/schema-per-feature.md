---
title: Schema-per-Feature
type: concept
tags: [persistence, ef-core, postgres, ddd]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

Every feature slice owns its own EF Core `DbContext` and its own Postgres
schema, but they all share a single physical database (`appdb`). This keeps
deployment simple (one connection string, one backup) while enforcing logical
isolation that makes future extraction into separate services cheap.

## How it works

- Each `Features/<Name>/Infrastructure/<Name>DbContext.cs` declares its schema
  in `OnModelCreating`.
- Schemas in use: `catalog`, `cart`, `ordering`, `inventory`, `profiles`,
  `reviews`, `wishlists`, plus a shared `outbox` for MassTransit's transactional
  outbox.
- Migrations are per-feature (under each slice's `Infrastructure/Migrations/`),
  not global.
- All contexts inherit from `BaseDbContext`, which wires the audit/concurrency/
  soft-delete/domain-event interceptors and applies `UseSnakeCaseNamingConvention()`.

## Why this and not separate databases

- Local dev and integration tests stay fast — one Postgres container, not eight.
- Cross-slice queries are still impossible in code (each slice only sees its own
  context), so the boundary is enforced where it matters.
- Lifting a slice out later means: point its `DbContext` at a new database,
  copy the schema, repoint the connection string. No code change inside the slice.

## Trade-offs

- A single `DROP DATABASE` wipes everything. Backup strategy must treat the
  whole `appdb` as one unit.
- A long-running migration on one schema can lock shared catalog tables in
  Postgres if you're not careful.

## Cross-references

- [[api-service]]
- [[cqrs-mediatr]] — handlers depend on the per-slice DbContext
- [[checkout-saga]] — saga state lives in the `ordering` schema, outbox in `outbox`

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md) — "Schema-per-Feature: All DbContexts share one `appdb` PostgreSQL database with separate schemas"
