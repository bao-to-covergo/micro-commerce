---
title: Dapr Pub/sub Namespace Consumer Groups (source)
type: source
tags: [dapr, pubsub, namespace, multi-tenancy]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/howto-namespace.md
---

# Dapr Pub/sub Namespace Consumer Groups (source)

Setting `consumerID: "{namespace}"` makes Dapr substitute the runtime
namespace into the broker's consumer group name. Lets the same `app-id` in
different namespaces consume independently — enables blue/green, canary, and
A/B without renaming apps.

## Key takeaways

- Without `{namespace}`, namespaced apps with the same `app-id` collide on
  the broker's consumer group.
- The substitution happens dynamically at runtime — no app code changes.
- Can be added retroactively to existing deployments.

## Verbatim quotes

- "Dapr understands the namespace it is running in and completes the
  namespace value for you, like a dynamic metadata value injected by the
  runtime."
- "If you add the namespace consumer group to your metadata afterwards,
  Dapr updates everything for you."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/howto-namespace.md`
