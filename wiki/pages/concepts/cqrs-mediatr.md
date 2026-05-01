---
title: CQRS with MediatR
type: concept
tags: [cqrs, mediatr, validation, fluentresults]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

Inside each feature slice, write operations are commands and read operations
are queries — both are MediatR `IRequest<T>` records dispatched through a
shared pipeline. Handlers are thin: validate input, run domain logic on
aggregates, return either a DTO or a `Result<T>`.

## Pipeline behaviors

Two behaviors run around every handler, defined in
`Common/Behaviors/`:

1. **`ValidationBehavior`** — runs FluentValidation validators registered for
   the request type. Validation failures short-circuit the pipeline before the
   handler executes.
2. **`ResultValidationBehavior`** — when a handler returns a FluentResults
   `Result<T>`, this behavior translates `IsFailed` outcomes into 422 responses
   via the FluentResults → HTTP mapper in `Common/Extensions/`.

## Conventions

- Commands and queries are records (`public sealed record CreateOrderCommand(...)`).
- Handlers live next to their request under `Application/Commands/` or
  `Application/Queries/`.
- I/O is always async; no `.Result` / `.Wait()` (enforced by the project's
  C# conventions).
- Domain events are raised on aggregates and published *after* `SaveChanges`
  by `DomainEventInterceptor`, so a failed transaction never leaks events.

## Cross-references

- [[api-service]] — hosts the MediatR registration and pipeline
- [[schema-per-feature]] — handlers depend on the slice's owned DbContext
- [[checkout-saga]] — saga consumes commands/events produced by handlers

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md) — "Commands and queries via MediatR with pipeline behaviors (ValidationBehavior, ResultValidationBehavior)"
