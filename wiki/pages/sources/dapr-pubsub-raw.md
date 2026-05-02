---
title: Dapr Pub/sub Raw Mode (source)
type: source
tags: [dapr, pubsub, raw, interop]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-raw.md
---

# Dapr Pub/sub Raw Mode (source)

Raw mode bypasses the CloudEvents envelope on publish (`rawPayload: true`) or
subscribe (`isRawPayload: true`). Useful for interop with non-Dapr components,
at the cost of tracing, dedup, and schema features. Subscribed raw messages
are still re-wrapped into a CloudEvent (base64 + `application/octet-stream`)
before delivery to the app.

## Key takeaways

- Raw publish: `rawPayload: true` in publish metadata.
- Raw subscribe: `isRawPayload: true` in subscription metadata
  (.NET uses `isRawPayload`).
- Disables CloudEvent-derived features: tracing, message-id dedup,
  content-type metadata.
- Useful when interoperating with non-Dapr publishers/subscribers.

## Verbatim quotes

- "Not using CloudEvents disables support for tracing, event deduplication
  per messageId, content-type metadata, and any other features built using
  the CloudEvent schema."
- "However, the subscribing Dapr process still wraps these raw messages in a
  CloudEvent before delivering them to the subscribing application."
- "When using raw payloads the message is always base64 encoded with content
  type `application/octet-stream`."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-raw.md`
