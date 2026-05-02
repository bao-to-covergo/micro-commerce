# Wiki Index

Catalog of every page in the wiki. The LLM updates this on every ingest and
rebuilds it during a lint pass. Read this first when answering a query.

Categories: **Entities** (concrete things), **Concepts** (ideas/patterns),
**Sources** (one summary per item in `raw/`), **Analyses** (synthesis pages
filed back from queries).

Current scope: **Dapr documentation** (pub/sub, workflow, jobs/scheduler,
secrets). See `AGENTS.md` *Scope* section for details. Pre-existing
MicroCommerce seed pages are retained under "Legacy" until explicitly
removed.

## Entities

- [Dapr Scheduler Service](pages/entities/dapr-scheduler-service.md) — control-plane that persists and triggers Jobs, actor reminders, and workflow timers via embedded etcd

## Concepts

- [Dapr Pub/Sub](pages/concepts/dapr-pubsub.md) — at-least-once messaging API with CloudEvents envelope, three subscription types, and DLQ on every component
- [Dapr Workflow](pages/concepts/dapr-workflow.md) — durable orchestration with deterministic code, replay-based recovery, and at-least-once activity execution
- [Dapr Jobs](pages/concepts/dapr-jobs.md) — durable scheduled callbacks (cron / duration / RFC3339) backed by the Scheduler service
- [Dapr Secrets](pages/concepts/dapr-secrets.md) — uniform `getSecret`/`getBulkSecret` API over pluggable backends with scope-based access control

## Sources

### Dapr — Pub/Sub

- [Pub/sub overview](pages/sources/dapr-pubsub-overview.md) — at-least-once guarantee, three subscription types, competing consumers
- [Publish & subscribe how-to](pages/sources/dapr-pubsub-howto.md) — component YAML, declarative subscription, HTTP/SDK publish flow
- [CloudEvents envelope](pages/sources/dapr-pubsub-cloudevents.md) — auto-filled fields, `cloudevent.*` overrides, binary mode
- [Subscription methods](pages/sources/dapr-pubsub-subscription-methods.md) — declarative vs programmatic vs streaming comparison
- [Dead-letter topics](pages/sources/dapr-pubsub-deadletter.md) — universal DLQ support, pair with retry policy
- [Message TTL](pages/sources/dapr-pubsub-ttl.md) — `ttlInSeconds` per-message TTL, native-broker forwarding
- [Bulk publish/subscribe](pages/sources/dapr-pubsub-bulk.md) — batched throughput, per-entry status, broker-level optimization matrix
- [Topic scoping](pages/sources/dapr-pubsub-scopes.md) — `publishingScopes`, `subscriptionScopes`, `protectedTopics`
- [Raw mode](pages/sources/dapr-pubsub-raw.md) — bypass CloudEvents for non-Dapr interop, with caveats
- [Content-based routing](pages/sources/dapr-pubsub-routing.md) — CEL `match` expressions on CloudEvent fields
- [Namespace consumer groups](pages/sources/dapr-pubsub-namespace.md) — `consumerID: "{namespace}"` substitution
- [StatefulSet subscribers](pages/sources/dapr-pubsub-statefulset.md) — `consumerID: "{podName}"` for sticky offsets

### Dapr — Workflow

- [Workflow overview](pages/sources/dapr-workflow-overview.md) — SDK matrix, child workflows, CLI lifecycle
- [Workflow architecture](pages/sources/dapr-workflow-architecture.md) — actor types, state keys, reminder semantics
- [Features and concepts](pages/sources/dapr-workflow-features.md) — determinism rules, retry policy, external events
- [Patterns](pages/sources/dapr-workflow-patterns.md) — task chaining, fan-out/fan-in, async HTTP, monitor, external events, saga
- [Concurrency](pages/sources/dapr-workflow-concurrency.md) — `maxConcurrent*Invocations` per-sidecar caps
- [Versioning](pages/sources/dapr-workflow-versioning.md) — patching vs named versioning, stall semantics
- [History retention](pages/sources/dapr-workflow-retention.md) — `stateRetentionPolicy`, non-retroactive, purge
- [Multi-app workflows](pages/sources/dapr-workflow-multi-app.md) — cross-`appID` activities and child workflows
- [How-to: author a workflow](pages/sources/dapr-workflow-howto-author.md) — SDK registration, "I/O in activities" rule
- [How-to: manage workflows](pages/sources/dapr-workflow-howto-manage.md) — full CLI/SDK/HTTP lifecycle surface

### Dapr — Jobs & Scheduler

- [Jobs overview](pages/sources/dapr-jobs-overview.md) — scheduler-not-executor, at-least-once, never-before-due
- [Jobs features and concepts](pages/sources/dapr-jobs-features.md) — schedule syntax, trigger callback contract, CLI
- [Jobs how-to](pages/sources/dapr-jobs-howto.md) — .NET and Go SDK schedule + handler patterns
- [Scheduler service](pages/sources/dapr-scheduler-service.md) — peer architecture, etcd, retries, staging queue
- [Scheduler persistence on Kubernetes](pages/sources/dapr-scheduler-kubernetes-persistence.md) — PVC sizing, resize procedure, in-memory option
- [Scheduler persistence self-hosted](pages/sources/dapr-scheduler-self-hosted-persistence.md) — Docker volume `dapr_scheduler`

### Dapr — Secrets

- [Secrets overview](pages/sources/dapr-secrets-overview.md) — built-in K8s store, AKS pod identities, secretKeyRef pattern
- [Secrets how-to](pages/sources/dapr-secrets-howto.md) — local file store, HTTP shape, SDK `GetSecret`/`GetBulkSecret`
- [Secrets scoping](pages/sources/dapr-secrets-scopes.md) — `defaultAccess`, `allowedSecrets`, `deniedSecrets`, precedence

### Karpathy reference

- [Karpathy LLM Wiki gist](pages/sources/karpathy-llm-wiki.md) — the original pattern this wiki implements

## Analyses

_(none yet — file synthesis here when queries produce keepers)_

## Legacy (MicroCommerce seed)

Retained per the "don't delete without asking" rule. These pages predate the
Dapr scope and are no longer linked from the active concept/entity pages.

- [MicroCommerce Platform](pages/entities/microcommerce-platform.md)
- [ApiService](pages/entities/api-service.md)
- [YARP Gateway](pages/entities/yarp-gateway.md)
- [Schema-per-Feature](pages/concepts/schema-per-feature.md)
- [CQRS with MediatR](pages/concepts/cqrs-mediatr.md)
- [Checkout Saga](pages/concepts/checkout-saga.md)
- [MicroCommerce CLAUDE.md](pages/sources/microcommerce-claude-md.md)
