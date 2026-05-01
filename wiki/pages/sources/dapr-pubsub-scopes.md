---
title: Dapr Pub/sub Topic Scoping (source)
type: source
tags: [dapr, pubsub, scopes, multi-tenancy]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/pubsub/pubsub-scopes.md
---

# Dapr Pub/sub Topic Scoping (source)

Component-level access control: `publishingScopes`, `subscriptionScopes`,
`allowedTopics`, `protectedTopics`. Defaults are permissive — without scopes,
all apps can publish/subscribe to all topics. Empty value for an app = explicit
deny. Unlisted apps = full access unless `protectedTopics` denies by default.

## Key takeaways

- `app1=topic1;app2=topic2,topic3;app3=` — `app3` is denied all topics.
- `allowedTopics` is a global topic whitelist (prevents runaway topic
  creation).
- `protectedTopics` flips defaults to deny-by-default for those topics.

## Verbatim quotes

- "If nothing is specified in `publishingScopes` (default behavior), all
  apps can publish to all topics."
- "To deny an app the ability to publish to any topic, leave the topics
  list blank (`app1=;app2=topic2`)."
- "If a topic is marked as protected then an application must be explicitly
  granted publish or subscribe permissions through `publishingScopes` or
  `subscriptionScopes`."

## See also

- Concept: [[dapr-pubsub]]
- Raw: `wiki/raw/developing-applications/building-blocks/pubsub/pubsub-scopes.md`
