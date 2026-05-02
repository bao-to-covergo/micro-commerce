---
title: Dapr Workflow
type: concept
tags: [dapr, workflow, orchestration, durable-execution, saga]
created: 2026-05-01
updated: 2026-05-01
sources:
  - sources/dapr-workflow-overview.md
  - sources/dapr-workflow-architecture.md
  - sources/dapr-workflow-features.md
  - sources/dapr-workflow-patterns.md
  - sources/dapr-workflow-concurrency.md
  - sources/dapr-workflow-versioning.md
  - sources/dapr-workflow-retention.md
  - sources/dapr-workflow-multi-app.md
  - sources/dapr-workflow-howto-author.md
  - sources/dapr-workflow-howto-manage.md
---

# Dapr Workflow

A stateful, durable orchestration building block embedded in the Dapr sidecar.
Developers express long-running business processes as ordinary code in their
language of choice (Python, JavaScript, .NET, Java, Go); Dapr handles
checkpointing, recovery from crashes, and at-least-once activity execution
using [[dapr-actors]] and a configured state store. HTTP/gRPC APIs let any
client start, query, pause/resume, raise events on, terminate, and purge
workflow instances.

## How it works

The engine runs inside the sidecar as a `durabletask-go` library and talks to
your app over a single application-initiated gRPC stream. Per workflow app
Dapr auto-registers two internal actor types:

- `dapr.internal.{namespace}.{appID}.workflow` — stateful; per-instance keys
  are `inbox-NNNNNN`, `history-NNNNNN`, `customStatus`, `metadata`.
- `dapr.internal.{namespace}.{appID}.activity` — stateless, short-lived;
  IDs are `{workflowID}::{seq}::{generation}`, e.g. `876bf371::2::1`.

A third `executor` actor type appears under the Dapr Shared clustered-
deployment preview. Reminders are **one-shot** — they expire after successful
trigger; on crash, the reminder reactivates and execution is *"retried,
forever"*.

State stores supported for workflows: PostgreSQL, MySQL, SQL Server, SQLite,
Oracle, CockroachDB, MongoDB, Redis. Reminders themselves live in the
[[dapr-scheduler-service]]. Cosmos DB and DynamoDB cap items at 2 MB UTF-8 JSON
which limits activity input/output payloads.

## Authoring model

Workflows are code, not YAML or DSL. The orchestrator function schedules tasks
through the SDK context. There's one rule that drives everything else:

> **Workflow code must be deterministic.** Replay must reproduce the same
> output as the original execution.

That means **inside the workflow body**, do not call `DateTime.UtcNow`,
`Guid.NewGuid`, env vars, file/HTTP I/O, or spawn background threads. Use
`context.CurrentUtcDateTime`, `context.NewGuid`, etc. **Activities** are where
non-deterministic work belongs — they're unconstrained and execute
**at least once**, so make them idempotent.

```csharp
// don't (non-deterministic)
DateTime now = DateTime.UtcNow;
Guid id  = Guid.NewGuid();

// do (deterministic, replay-safe)
DateTime now = context.CurrentUtcDateTime;
Guid id  = context.NewGuid();
```

JavaScript-specific rule: never declare a workflow `async` — Node.js doesn't
guarantee async-function determinism.

## Five well-known patterns

| Pattern | Sketch |
|---|---|
| **Task chaining** | Sequential `await context.CallActivityAsync(A); await ...B; await ...C` — runtime auto-saves progress; on crash, resumes from last completed step. |
| **Fan-out/fan-in** | Schedule N tasks then `WhenAll` / `Task.WhenAll`. Cap concurrency with `MaxParallelism`. If 100 tasks fan out and 40 finish before crash, restart re-runs only the remaining 60. |
| **Async HTTP API** | Client POSTs to `/start`, polls GET `/<instanceID>` for `runtimeStatus` (`RUNNING`/`COMPLETED`/`FAILURE`/`TERMINATED`). Built into the engine — no extra plumbing. |
| **Monitor (eternal)** | Recurring poll/sleep via `CreateTimer` + `ContinueAsNew` to avoid `while(true)` and unbounded history. |
| **External events** | `WaitForExternalEvent("name", timeout)` blocks until `RaiseEvent` arrives. Timeout surfaces as `TaskCanceledException` (.NET/Java). FIFO dispatch for same-name events. |

A sixth, **compensation / saga**, is implemented by tracking completed steps
in workflow state and executing compensating activities in reverse order on
failure — no framework dependency required.

## Lifecycle / management

Three surfaces, all equivalent:

```bash
# CLI
dapr workflow run MyWorkflow --app-id myapp --input '{"key": "value"}'
dapr workflow list --app-id myapp --filter-status RUNNING
dapr workflow suspend <id> --app-id myapp
dapr workflow purge --app-id myapp --all-older-than 720h
```

```bash
# HTTP — component name is literally "dapr"
POST /v1.0/workflows/dapr/<workflowName>/start?instanceID=<id>
POST /v1.0/workflows/dapr/<id>/pause
POST /v1.0/workflows/dapr/<id>/resume
POST /v1.0/workflows/dapr/<id>/raiseEvent/<eventName>
POST /v1.0/workflows/dapr/<id>/terminate
POST /v1.0/workflows/dapr/<id>/purge
GET  /v1.0/workflows/dapr/<id>
```

SDKs expose the equivalent client methods (`ScheduleNewWorkflow`,
`PauseWorkflow`, `RaiseEvent`, `Terminate`, `Purge`,
`WaitForWorkflowCompletion`).

Instance IDs allow only alphanumerics, `_`, and `-`. Purge requires the
workflow to be in `COMPLETED`, `FAILED`, or `TERMINATED` and a running
workflow client; `--force` bypasses the check but **risks state-machine
corruption**.

## History & retention

History is appended event-sourced. Records per task: start=5, activity=3,
timer=3, raise-event=3, child-workflow=8.

Default retention is **indefinite** — completed workflow state stays in the
state store forever unless cleaned up. Configure cleanup with:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: appconfig
spec:
  workflow:
    stateRetentionPolicy:
      anyTerminal: "360h"
      completed: "1m"
      failed: "720h"
      terminated: "360h"
```

The policy is **not retroactive** — only applies to workflows that reach a
terminal state after it's in effect. To clean up existing terminal workflows,
run `dapr workflow purge --all-older-than <duration>`.

## Concurrency

Per-sidecar caps configured on the same Configuration:

```yaml
spec:
  workflow:
    maxConcurrentWorkflowInvocations: 100
    maxConcurrentActivityInvocations: 1000
```

> "These limits are imposed on a per sidecar basis, meaning that if you have
> 10 replicas of your workflow app, the effective limit is 10 times the
> configured value."

Both default to **infinite**. Limits apply across all workflow/activity
definitions — no per-name granularity. All replicas must register the **exact
same** set of workflows and activities.

## Versioning

Use the two strategies in conjunction:

- **Patching** — `if (context.IsPatched("identifier")) { ... }`. Identifiers
  are stored in history; they must be unique per workflow, never reused across
  deployments, and applied additively. Original code must remain unchanged.
  Mismatches stall workflows.
- **Named versioning** — duplicate the workflow as `WorkflowV2`, register with
  `isLatest=true`; runtime routes new instances to the latest, existing
  instances continue on their original version. **No auto-migration.**

```go
registry := workflow.NewRegistry()
registry.AddVersionedWorkflow("Workflow", false, WorkflowV1)
registry.AddVersionedWorkflow("Workflow", true,  WorkflowV2)
```

Stalls during rollout (e.g. `VERSION_NAME_MISMATCH;description=Version not
available: workflow_v1`) clear once only new replicas remain. Workflow
input/output schema changes are **not** covered by versioning — use serialized
strings or optional fields.

## Multi-app workflows

Activities and child workflows can target a different `appID` (e.g. ML-on-GPU,
data residency, language boundary). Parent owns the history and the final
result returns to it.

```csharp
var options = new WorkflowTaskOptions { TargetAppId = "App2" };
var output  = await context.CallActivityAsync<string>(nameof(ActivityA), input, options);
```

Restrictions:

- Same namespace.
- Same actor state store.
- Missing target activity → parent retries **indefinitely**.
- Missing target app → retries per retry policy.

Requires Dapr ≥ 1.16 (.NET SDK ≥ 1.17). Full SDK support: Go, Python, .NET.
Java supports activities only. JavaScript planned.

Dapr 1.17 adds the `WorkflowsRemoteActivityReminder` feature gate (off by
default) — delivers activity results via reminder when the owning workflow
app is offline, preventing duplicate execution.

## Cross-references

- [[dapr-jobs]] — workflows internally schedule activity reminders via the
  same Scheduler service
- [[dapr-scheduler-service]] — control-plane storing workflow reminders
- [[dapr-pubsub]] — workflows often publish lifecycle events as activities
- [[dapr-secrets]] — credentials for activities (DB, HTTP) are loaded via
  the secrets API

## Sources

- [Workflow overview](../sources/dapr-workflow-overview.md) — backs SDK
  matrix, child workflow termination, and CLI surface.
- [Workflow architecture](../sources/dapr-workflow-architecture.md) — backs
  the actor types, state keys, reminder semantics, and state-store list.
- [Features and concepts](../sources/dapr-workflow-features.md) — backs the
  determinism rules, retry policy, and external-event semantics.
- [Workflow patterns](../sources/dapr-workflow-patterns.md) — backs the five
  patterns and the runtime-status enum.
- [Concurrency](../sources/dapr-workflow-concurrency.md) — backs the
  per-sidecar default-infinite caps.
- [Versioning](../sources/dapr-workflow-versioning.md) — backs patching vs
  named versioning and the stall semantics.
- [History retention](../sources/dapr-workflow-retention.md) — backs default
  retention is indefinite and the not-retroactive caveat.
- [Multi-app workflows](../sources/dapr-workflow-multi-app.md) — backs the
  cross-app routing options and the SDK support matrix.
- [How to: author a workflow](../sources/dapr-workflow-howto-author.md) —
  backs the SDK registration pattern and "no I/O in workflow body" rule.
- [How to: manage workflows](../sources/dapr-workflow-howto-manage.md) —
  backs the HTTP/CLI lifecycle endpoints and purge restrictions.
