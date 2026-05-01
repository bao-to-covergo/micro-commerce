---
title: Dapr Pub/sub Content-Based Routing (source)
type: source
tags: [dapr, pubsub, routing, cel]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/howto-route-messages.md
---

# Dapr Pub/sub Content-Based Routing (source)

Routes CloudEvents to different paths via CEL `match` expressions on event
fields, with an optional `default` route. Available for declarative and
programmatic subscriptions; **not** for streaming.

## Key takeaways

- `routes.rules[]` with `match` (CEL) and `path`. Optional `default`.
- Numeric values default to floats — cast with `int(...)`.
- `event` is the CloudEvent root; `event.data.*` accesses payload fields.
- Time-relative comparisons not supported.
- Nested data access requires un-escaped JSON.

## Verbatim quotes

- "This feature is available to both the declarative and programmatic
  subscription approaches, however does not apply to streaming
  subscriptions."
- "By default the numeric values are written as double-precision
  floating-point. There are no automatic arithmetic conversions."
- "Currently, you can only access the attributes inside data if it is
  nested JSON values and not JSON escaped in a string."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/howto-route-messages.md`
