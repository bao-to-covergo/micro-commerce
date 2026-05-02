---
title: Dapr Pub/sub with CloudEvents (source)
type: source
tags: [dapr, pubsub, cloudevents]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-cloudevents.md
---

# Dapr Pub/sub with CloudEvents (source)

Explains why Dapr wraps messages in CloudEvents (tracing, content-type-aware
deserialization, sender verification), the auto-generated envelope fields,
how to override specific fields with `cloudevent.*` publish metadata, how to
publish a fully custom envelope (`Content-Type: application/cloudevents+json`),
and binary CloudEvents via `application/octet-stream` + `ce_*` headers.

## Key takeaways

- Auto-filled envelope fields: `id`, `source`, `specversion`, `type`,
  `traceparent`, `traceid`, `tracestate`, `topic`, `pubsubname`, `time`,
  `datacontenttype` (optional).
- Override metadata keys: `cloudevent.id`, `cloudevent.source`,
  `cloudevent.type`, `cloudevent.traceid`, `cloudevent.tracestate`,
  `cloudevent.traceparent`. Apply to all pub/sub components.
- Fully custom envelope must satisfy CloudEvents minimum required fields or
  the message is rejected.
- Binary mode: body is raw bytes; attributes ride on transport headers
  prefixed with `ce_`.
- Dapr does not deduplicate by `id`; brokers with native dedup can be used.

## Verbatim quotes

- "While you can replace `traceid`/`traceparent` and `tracestate`, doing this
  may interfere with tracing events and report inconsistent results in
  tracing tools. It's recommended to use Open Telemetry for distributed
  traces."
- "Dapr does not handle deduplication automatically. Dapr supports using
  message brokers that natively enable message deduplication."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-cloudevents.md`
