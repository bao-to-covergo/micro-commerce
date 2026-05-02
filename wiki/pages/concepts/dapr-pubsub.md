---
title: Dapr Pub/Sub
type: concept
tags: [dapr, pubsub, messaging, cloudevents, event-driven]
created: 2026-05-01
updated: 2026-05-01
sources:
  - sources/dapr-pubsub-overview.md
  - sources/dapr-pubsub-howto.md
  - sources/dapr-pubsub-cloudevents.md
  - sources/dapr-pubsub-subscription-methods.md
  - sources/dapr-pubsub-deadletter.md
  - sources/dapr-pubsub-ttl.md
  - sources/dapr-pubsub-bulk.md
  - sources/dapr-pubsub-scopes.md
  - sources/dapr-pubsub-raw.md
  - sources/dapr-pubsub-routing.md
  - sources/dapr-pubsub-namespace.md
  - sources/dapr-pubsub-statefulset.md
---

# Dapr Pub/Sub

A platform-agnostic publish/subscribe API exposed by the Dapr sidecar that
brokers messages between services through a pluggable component (Redis Streams,
Kafka, RabbitMQ, Azure Service Bus, NATS, GCP Pub/Sub, etc.). Apps publish to a
named topic; Dapr delivers each message at-least-once to every subscribing app,
wrapping it in a CloudEvents 1.0 envelope by default.

## How it works

1. The app calls `POST /v1.0/publish/<pubsub-name>/<topic>` on its sidecar.
2. The sidecar wraps the body in a CloudEvent envelope and forwards it to the
   broker via the configured pub/sub component.
3. Subscriber sidecars receive messages from the broker on behalf of the app
   and dispatch them to an HTTP route or stream handler defined by one of three
   subscription methods.
4. Returning `200 OK` (or the equivalent gRPC ACK) marks the message delivered.
   Anything else (or a crash) triggers redelivery — Dapr guarantees
   **at-least-once** semantics on every component.

## Subscription methods

| Method | Where defined | Dynamic? | Routes? | Bulk? | Endpoint required? |
|---|---|---|---|---|---|
| **Declarative** | YAML `kind: Subscription` (`apiVersion: dapr.io/v2alpha1`) | Hot-reload preview only | yes | yes | yes |
| **Programmatic** | App code (e.g. `[Topic]` attr, `/dapr/subscribe`) | no — read once at startup | yes | yes | yes |
| **Streaming** | SDK pull stream (`SubscribeAsync`, `subscribe`) | yes — start/stop at runtime | no | no | no — works without a sidecar app at all |

Declarative is the default recommendation: it removes Dapr from app code,
survives moves between SDKs, and is the only style that supports CRD-style
deployment in Kubernetes. Streaming wins when you need dynamic
subscribe/unsubscribe or a worker that consumes without exposing HTTP.

## CloudEvents envelope

Every message is wrapped automatically. Dapr fills in `id`, `source`,
`specversion`, `type`, `traceparent`/`traceid`/`tracestate`, `topic`,
`pubsubname`, `time`, and (when known) `datacontenttype`. Apps may override
specific fields via publish metadata (`cloudevent.id`, `cloudevent.source`,
`cloudevent.type`, …) or supply a fully custom envelope by setting
`Content-Type: application/cloudevents+json`. Binary CloudEvents are also
supported via `application/octet-stream` body + `ce_*` transport headers.

CloudEvents enable **distributed tracing**, content-type-aware deserialization,
and sender verification. Disabling them (raw mode) loses those features.

## Raw mode (no CloudEvent)

Set `rawPayload: true` on publish or `isRawPayload: true` on subscription
metadata to interop with non-Dapr publishers/subscribers. Caveat: even in raw
mode the **subscribing** sidecar still wraps messages into a CloudEvent before
delivering to your app, with the body base64-encoded as
`application/octet-stream`. Disables tracing, message-id dedup, and content
type metadata.

## Reliability features

- **Dead-letter topics** — `deadLetterTopic` on a subscription forwards
  undeliverable messages to a sidecar topic. Available on **every** Dapr
  component regardless of native broker support. Without a Resiliency `retries`
  policy, the very first failure goes to the DLQ — pair them.
- **Per-message TTL** — publish metadata `ttlInSeconds`. Enforced at the Dapr
  runtime layer so it works on every component; native-TTL brokers (Azure
  Service Bus) get the value forwarded and use their own semantics. Non-Dapr
  subscribers must read the CloudEvent `expiration` (RFC3339) themselves.
- **Subscription startup retries** — Dapr retries failed subscription startup
  (e.g. transient broker auth/network errors) and logs each attempt.

## Scoping & multi-tenancy

- **Topic scoping** — `publishingScopes`, `subscriptionScopes`, `allowedTopics`,
  `protectedTopics` on the component metadata gate which apps may publish or
  subscribe to which topics. Defaults are permissive; `protectedTopics` flips
  to deny-by-default for specific topic names.
- **Namespace consumer groups** — set `consumerID: "{namespace}"` and Dapr
  substitutes the runtime namespace at startup, so the same `app-id` in
  different namespaces gets distinct consumer groups (enables blue/green,
  canary, A/B without renaming apps).
- **StatefulSet pod identity** — `consumerID: "{podName}"` gives each replica a
  stable consumer ID. Brokers like Kafka keep per-pod offsets across restarts
  so no messages are skipped on rolling updates.

## Bulk and routing

- **Bulk publish/subscribe** — batches many messages per request. Bulk publish
  is non-transactional (per-entry status: `SUCCESS`, `RETRY`, `DROP`; missing
  status = `RETRY`). Bulk subscribe uses `bulkSubscribe.enabled: true` with
  `maxMessagesCount` and `maxAwaitDurationMs`. App↔Dapr optimization is
  universal; Dapr↔broker batching exists for Kafka, Azure Service Bus, and
  Azure Event Hubs. Streaming subscriptions cannot bulk subscribe.
- **Content-based routing** — `routes.rules[].match` uses CEL expressions
  (`event.type == "widget"`, `event.data.amount > 100.0`) to dispatch to
  different paths with an optional `default`. Available for declarative and
  programmatic, **not** streaming. Numerics are floats by default — cast with
  `int(...)`. No time-relative comparisons; nested data access requires
  un-escaped JSON.

## Outbox pattern

Dapr supports the transactional outbox over a state store + pub/sub
combination, atomically committing state changes and the messages that
announce them. See the upstream "How-To: Enable transactional outbox messaging"
doc — not yet ingested into this wiki.

## Competing-consumer pattern

Replicas of an app sharing an `app-id` form a single consumer group; Dapr
delivers each message to **exactly one** replica. Two different apps subscribed
to the same topic each get their own copy. Native support varies by broker —
explicitly listed: Apache Kafka, Azure Service Bus Queues, RabbitMQ, Redis
Streams.

## Component example

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: order-pub-sub
spec:
  type: pubsub.rabbitmq
  version: v1
  metadata:
    - name: connectionString
      value: "amqp://localhost:5672"
    - name: durable
      value: "false"
scopes:
  - orderprocessing
  - checkout
```

## Cross-references

- [[dapr-runtime]] — sidecar that exposes the pub/sub API
- [[dapr-secrets]] — used to inject broker credentials into pub/sub
  components via `secretKeyRef`
- [[dapr-workflow]] — workflows can publish events as activities

## Sources

- [Dapr Pub/sub overview](../sources/dapr-pubsub-overview.md) — backs the
  at-least-once guarantee and the three subscription types.
- [Publish & subscribe how-to](../sources/dapr-pubsub-howto.md) — backs the
  HTTP/SDK call shapes and the component YAML.
- [CloudEvents in pub/sub](../sources/dapr-pubsub-cloudevents.md) — backs the
  envelope fields, override metadata, and binary mode.
- [Subscription methods](../sources/dapr-pubsub-subscription-methods.md) —
  backs the comparison table.
- [Dead-letter topics](../sources/dapr-pubsub-deadletter.md) — backs DLQ
  universal support and the retry-policy pairing.
- [Message TTL](../sources/dapr-pubsub-ttl.md) — backs per-message TTL and
  the non-Dapr-subscriber caveat.
- [Bulk pub/sub](../sources/dapr-pubsub-bulk.md) — backs bulk semantics and
  the limited Dapr↔broker optimization list.
- [Topic scoping](../sources/dapr-pubsub-scopes.md) — backs `publishingScopes`
  / `subscriptionScopes` / `protectedTopics` rules.
- [Raw mode](../sources/dapr-pubsub-raw.md) — backs the raw-mode caveats.
- [Content-based routing](../sources/dapr-pubsub-routing.md) — backs CEL
  expression support and the `int()` cast caveat.
- [Namespace consumer groups](../sources/dapr-pubsub-namespace.md) — backs
  `{namespace}` substitution.
- [StatefulSet subscribers](../sources/dapr-pubsub-statefulset.md) — backs
  `{podName}` substitution.
