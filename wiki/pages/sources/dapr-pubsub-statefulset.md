---
title: Dapr Pub/sub Subscribers via StatefulSet (source)
type: source
tags: [dapr, pubsub, statefulset, kafka, scaling]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/howto-subscribe-statefulset.md
---

# Dapr Pub/sub Subscribers via StatefulSet (source)

Kubernetes StatefulSet pods get sticky identities, so using
`consumerID: "{podName}"` gives each replica a stable consumer ID. Brokers
like Kafka keep per-pod offsets across restarts so no messages are skipped on
rolling updates.

## Key takeaways

- `{podName}` substitution is Dapr-native; no app code change needed.
- Kafka isolates each subscriber by `consumerID`; restarted pods resume from
  last known offset.
- MQTT3 shared topics let pods compete (one delivery), enabling shared work.
- Two consumption models: broadcast (all subscribers receive) vs shared
  (one receives) — broker-determined.

## Verbatim quotes

- "Dapr keeps track of the name of each Pod, which can be used when
  declaring components using the `{podName}` marker."
- "Kafka isolates each subscriber by `consumerID` with its own position in
  the topic. When an instance restarts, it reuses the same `consumerID` and
  continues from its last known position, without skipping messages."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/howto-subscribe-statefulset.md`
