---
title: "Source: MicroCommerce CLAUDE.md"
type: source
tags: [project-docs, conventions, architecture]
created: 2026-05-01
updated: 2026-05-01
raw: ../../../CLAUDE.md
---

The project's root `CLAUDE.md` — the single canonical document describing the
MicroCommerce platform's tech stack, project structure, conventions, and the
key patterns in use. It is the seed source for almost every page in this wiki.

## Key takeaways

- **Topology**: Aspire orchestrates Postgres → ApiService → YARP Gateway →
  Next.js frontend. Frontend never bypasses the gateway.
- **Architecture style**: modular monolith, vertical-slice features, intentionally
  shaped for later extraction into microservices.
- **Persistence**: schema-per-feature inside one shared `appdb` Postgres
  database; EF Core with snake_case convention; per-feature migrations.
- **Application pattern**: CQRS via MediatR with `ValidationBehavior` and
  `ResultValidationBehavior` in the pipeline.
- **Messaging**: MassTransit on Azure Service Bus (dev) or RabbitMQ (k8s);
  transactional outbox in its own schema; circuit breaker + exponential retry.
- **Cross-cutting**: Vogen strongly-typed IDs, Ardalis.SmartEnum domain enums,
  Ardalis.Specification for queries, FluentValidation for input, FluentResults
  for railway-style error returns.
- **Conventions**: file-scoped namespaces, primary constructors for DI,
  explicit types over `var`, records for DTOs/commands/queries, UUID v7 for
  new entity IDs, `TreatWarningsAsErrors`, nullable reference types on.
- **Frontend**: Next.js 16 + React 19 + TS strict, Tailwind v4, TanStack Query,
  Server Components by default, Biome for linting/formatting (kebab-case files,
  PascalCase components, `@/*` path alias).
- **Testing**: xUnit + Testcontainers (real Postgres) + MassTransit
  TestFramework for the backend; Playwright E2E for the frontend.
- **Auth**: Keycloak realm config under `MicroCommerce.AppHost/Realms/`;
  NextAuth.js v5 on the frontend.

## Pages derived from this source

- [[../entities/microcommerce-platform]]
- [[../entities/api-service]]
- [[../entities/yarp-gateway]]
- [[../concepts/schema-per-feature]]
- [[../concepts/cqrs-mediatr]]
- [[../concepts/checkout-saga]]

## Raw

`../../../CLAUDE.md` (repo root). Treat as immutable for wiki purposes — it is
maintained as project documentation, not as a wiki source per se, but the
content is canonical.
