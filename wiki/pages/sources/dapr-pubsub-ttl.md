---
title: Dapr Pub/sub Message TTL (source)
type: source
tags: [dapr, pubsub, ttl]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-message-ttl.md
---

# Dapr Pub/sub Message TTL (source)

Per-message TTL configured via the `ttlInSeconds` publish metadata. Dapr
enforces TTL in the runtime layer so it works on every component; native-TTL
brokers (e.g. Azure Service Bus) get the value forwarded and use their own
semantics. Non-Dapr subscribers must read the CloudEvent `expiration` field
themselves.

## Key takeaways

- `ttlInSeconds` is a publish-time metadata, not a component or subscription
  property.
- Native-TTL components: Dapr forwards rather than re-implements (preserves
  broker-specific behaviors like Service Bus's expired-to-DLQ move).
- Kafka topic-level `retention.ms` is independent of Dapr TTL.
- Non-Dapr consumers see the CloudEvent `expiration` (RFC3339) but won't
  auto-drop — they must check it.

## Verbatim quotes

- "All Dapr pub/sub components are compatible with message TTL, as Dapr
  handles the TTL logic within the runtime."
- "When message time-to-live has native support in the pub/sub component,
  Dapr simply forwards the time-to-live configuration without adding any
  extra logic."
- "If messages are consumed by subscribers not using Dapr, the expired
  messages are not automatically dropped."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-message-ttl.md`
