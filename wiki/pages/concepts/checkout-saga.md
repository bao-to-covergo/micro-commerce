---
title: Checkout Saga
type: concept
tags: [saga, masstransit, ordering, inventory, payments]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

The checkout flow is orchestrated by `CheckoutStateMachine`, a MassTransit
state machine living in `Features/Ordering/Application/Saga/`. It coordinates
the cross-slice steps that turn a cart into a confirmed order: reserve stock
in **Inventory**, charge the customer via **Payments**, then confirm or fail
the order in **Ordering**.

## State machine outline

```
[Started]
   │ OrderPlaced
   ▼
[AwaitingStockReservation]
   │ StockReserved      │ StockReservationFailed
   ▼                    ▼
[AwaitingPayment]      [Failed]
   │ PaymentSucceeded   │ PaymentFailed
   ▼                    ▼
[Confirmed]            [Failed → compensate: ReleaseStock]
```

State is persisted in the `ordering` schema via EF Core, so saga instances
survive restarts.

## Reliability building blocks

- **Transactional outbox** — `OutboxDbContext` in the `outbox` schema gives
  inbox deduplication and outbox delivery. Domain events are published only
  after the database transaction commits.
- **Retries + circuit breaker** — global MassTransit config: exponential
  backoff `[1s, 5s, 25s]`, circuit breaker tripping at 15% failure rate.
- **Dead letter queue** — `DeadLetterQueueService` exposes failed messages
  through the **Messaging** feature for manual replay.
- **Transport** — Azure Service Bus locally (via the emulator under Aspire),
  RabbitMQ in Kubernetes; selection is conditional in `Program.cs`.

## Cross-references

- [[api-service]] — hosts the saga
- [[cqrs-mediatr]] — saga reacts to events emitted by command handlers
- [[schema-per-feature]] — saga state in `ordering`, outbox in `outbox`

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md) — "Checkout Saga: CheckoutStateMachine orchestrates stock reservation -> payment -> confirm/fail with EF Core-persisted state"
