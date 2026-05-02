---
title: Dapr Pub/sub Overview (source)
type: source
tags: [dapr, pubsub]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-overview.md
---

# Dapr Pub/sub Overview (source)

Upstream landing concept page for the pub/sub building block. Frames pub/sub
as the platform-agnostic publish/subscribe API exposed by the Dapr sidecar,
backed by a pluggable component, with at-least-once delivery and a
CloudEvents 1.0 envelope.

## Key takeaways

- Pub/sub API is platform-agnostic; the broker is configured as a Dapr
  pub/sub **component** (Redis Streams, NATS, Azure Service Bus, GCP, Kafka,
  RabbitMQ, etc.).
- Default behavior wraps messages in a CloudEvents 1.0 envelope.
- Three subscription types: declarative, streaming, programmatic.
- Hot reload of declarative subscriptions is a preview feature
  (`HotReload` feature gate).
- Message routing uses content-based rules (CEL).
- Dead-letter topics are universally supported across all components.
- Outbox pattern is supported (via state store + pub/sub combo).
- Namespace consumer groups via `{namespace}` substitution.

## Verbatim quotes

- "Dapr guarantees at-least-once semantics for message delivery. When an
  application publishes a message to a topic using the pub/sub API, Dapr
  ensures the message is delivered *at least once* to every subscriber."
- "All Dapr pub/sub components support the at-least-once guarantee."
- "When multiple instances of the same application (with same `app-id`)
  subscribe to a topic, Dapr delivers each message to *only one instance of
  **that** application*."
- Components listed as supporting the competing-consumer pattern: Apache
  Kafka, Azure Service Bus Queues, RabbitMQ, Redis Streams.

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-overview.md`
