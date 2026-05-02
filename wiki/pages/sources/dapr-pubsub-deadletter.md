---
title: Dapr Pub/sub Dead Letter Topics (source)
type: source
tags: [dapr, pubsub, dlq, resiliency]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-deadletter.md
---

# Dapr Pub/sub Dead Letter Topics (source)

Documents Dapr's universal DLQ support — all components get DLQ behavior even
when the broker has no native equivalent — configured via `deadLetterTopic`
on the subscription. Strongly recommends pairing DLQ with a Resiliency retry
policy or first failure goes straight to the DLQ.

## Key takeaways

- DLQ supported on all Dapr pub/sub components (declarative, programmatic,
  streaming).
- Without a retry policy: first failure ⇒ DLQ. Pair with `pubsubRetry` policy.
- DLQ requires its own subscription to consume.
- Some components (AWS SNS/SQS, RabbitMQ) have native DLQ semantics that
  Dapr forwards to.

## Verbatim quotes

- "Dapr enables dead letter topics for all of it's pub/sub components, even
  if the underlying system does not support this feature natively."
- "By default, when a dead letter topic is set, any failing message
  immediately goes to the dead letter topic. As a result it is recommend to
  always have a retry policy set."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-deadletter.md`
