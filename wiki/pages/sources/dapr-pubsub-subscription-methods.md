---
title: Dapr Pub/sub Subscription Methods (source)
type: source
tags: [dapr, pubsub, subscriptions]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/subscription-methods.md
---

# Dapr Pub/sub Subscription Methods (source)

Defines the three ways a Dapr app can subscribe to topics — declarative,
programmatic, streaming — and contrasts them on routing, dynamism, and
endpoint requirements.

## Key takeaways

- **Declarative** — external `apiVersion: dapr.io/v2alpha1` Subscription
  resource. No Dapr in app code. Hot reload preview-gated.
- **Programmatic** — defined in app code (e.g. `[Topic]` attr) or via
  `GET /dapr/subscribe`. Static — read once at startup.
- **Streaming** — pull-based via SDK gRPC stream. Dynamic at runtime. No
  HTTP endpoint required; works with no app configured on the sidecar.
- Streaming has no concept of routes or bulk subscribe — messages flow to a
  handler.

## Verbatim quotes

- "no endpoint is needed to subscribe to a topic, and it's possible to
  subscribe without any app configured on the sidecar at all" (streaming).
- "Programmatic subscriptions are only read once during application start-up.
  You cannot _dynamically_ add new programmatic subscriptions."
- "in-flight messages between Dapr and your application are unaffected
  during hot reload events."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/subscription-methods.md`
