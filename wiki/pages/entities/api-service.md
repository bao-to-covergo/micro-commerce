---
title: ApiService
type: entity
tags: [backend, dotnet, aspnetcore, modular-monolith]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

`MicroCommerce.ApiService` is the single ASP.NET Core process that hosts every
feature slice. It is a modular monolith — one deployable, but each feature
owns its own `DbContext`, schema, and message contracts so individual slices
can be lifted out into separate services later without rewrites.

## Stack

- .NET 10, ASP.NET Core Minimal APIs
- MediatR for [[cqrs-mediatr]]; pipeline behaviors `ValidationBehavior`,
  `ResultValidationBehavior`
- EF Core with `UseSnakeCaseNamingConvention()` and per-slice DbContexts over
  one Postgres database (`appdb`, [[schema-per-feature]])
- MassTransit on Azure Service Bus (Aspire/dev) or RabbitMQ (Kubernetes),
  switched conditionally in `Program.cs`
- Vogen strongly-typed IDs, Ardalis.SmartEnum domain enums,
  Ardalis.Specification for queries, FluentValidation for input,
  FluentResults for railway-oriented errors

## Cross-cutting infrastructure

Lives under `src/MicroCommerce.ApiService/Common/`:

- **Behaviors** — MediatR pipeline (validation, result validation)
- **Persistence** — `BaseDbContext`, EF interceptors
  (`AuditInterceptor`, `ConcurrencyInterceptor`, `SoftDeleteInterceptor`,
  `DomainEventInterceptor`), conventions, and the MassTransit `OutboxDbContext`
- **Messaging** — `DeadLetterQueueService`, `DomainEventFaultConsumer`
- **Exceptions** — global exception handling
- **Extensions** — FluentResults → HTTP status mapping
- **OpenApi** — schema transformers for Vogen IDs and SmartEnums

## Cross-references

- [[microcommerce-platform]] — the overall system
- [[yarp-gateway]] — the only thing that should call this in prod
- [[cqrs-mediatr]]
- [[schema-per-feature]]
- [[checkout-saga]]

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md)
