---
title: Dapr Pub/sub Bulk Operations (source)
type: source
tags: [dapr, pubsub, bulk, throughput]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-bulk.md
---

# Dapr Pub/sub Bulk Operations (source)

Bulk publish/subscribe APIs reduce request count by batching. Bulk publish is
non-transactional with per-entry status. Bulk subscribe uses
`bulkSubscribe.enabled` plus `maxMessagesCount` and `maxAwaitDurationMs`. Not
supported by streaming subscriptions.

## Key takeaways

- App↔Dapr optimization is universal; Dapr↔broker optimization currently
  exists only for Kafka, Azure Service Bus, Azure Event Hubs.
- Per-entry status values: `SUCCESS`, `RETRY`, `DROP`. Missing status =
  `RETRY`.
- Each message has an `EntryId`; failures returned in `FailedEntries`.

## Verbatim quotes

- "It is *non-transactional*, i.e., from a single bulk request, some
  messages can succeed and some can fail."
- "Streaming - *Not supported* for bulk subscribe as messages are sent to
  handler code."
- "If the app fails to notify on an `EntryId` status, it's considered a
  `RETRY`."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-bulk.md`
