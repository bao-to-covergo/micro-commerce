---
title: Dapr Pub/sub How-To (source)
type: source
tags: [dapr, pubsub, howto]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/howto-publish-subscribe.md
---

# Dapr Pub/sub How-To (source)

Walkthrough that shows the end-to-end pub/sub flow: define a `pubsub.yaml`
component (defaults to Redis on `dapr init`; example uses RabbitMQ), define a
declarative `Subscription` resource, then publish via SDK or HTTP and consume
via SDK handlers in .NET, Java, Python, Go, JavaScript.

## Key takeaways

- `dapr init` writes a default Redis `pubsub.yaml` to `~/.dapr/components/`
  (Linux/macOS) or `%UserProfile%\.dapr\components\` (Windows).
- Declarative subscription uses `apiVersion: dapr.io/v2alpha1`,
  `kind: Subscription`, with `topic`, `routes.default`, `pubsubname`, and
  `scopes`.
- Returning `200 OK` ACKs the message; any other code (or a crash) triggers
  redelivery.
- Publish HTTP shape:
  `POST http://localhost:<dapr-http-port>/v1.0/publish/<pubsub-name>/<topic>`.
- CLI publish: `dapr publish --publish-app-id <id> --pubsub <name> --topic
  <t> --data '<json>'`.

## Verbatim quotes

- "In order to tell Dapr that a message was processed successfully, return a
  `200 OK` response. If Dapr receives any other return status code than
  `200`, or if your app crashes, Dapr will attempt to redeliver the message
  following at-least-once semantics."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/howto-publish-subscribe.md`
