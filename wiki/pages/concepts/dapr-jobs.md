---
title: Dapr Jobs
type: concept
tags: [dapr, jobs, scheduler, cron, scheduling]
created: 2026-05-01
updated: 2026-05-01
sources:
  - sources/dapr-jobs-overview.md
  - sources/dapr-jobs-features.md
  - sources/dapr-jobs-howto.md
  - sources/dapr-scheduler-service.md
---

# Dapr Jobs

The Jobs API is Dapr's application-facing **scheduler** building block: apps
register named jobs with a schedule (cron, duration, or RFC3339 datetime) and a
payload, and the Dapr [[dapr-scheduler-service]] control-plane service fires
them at the due time by calling back into the originating app's sidecar. Jobs
is "a job scheduler, not the executor" — design priority is **durability and
horizontal scaling over precision**.

## Job vs Scheduler

These are two halves of one feature:

- **Jobs API** — the per-app HTTP/gRPC/SDK surface for create, get, list,
  delete operations.
- **Scheduler service** — the control-plane process that persists job records
  in embedded etcd and fires triggers. Same service also powers actor
  reminders, workflow timers, and workflow activity reminders. Anything in
  `dapr scheduler list` is keyed by a type prefix: `app/`, `actor/`,
  `activity/`, `workflow/`.

See [[dapr-scheduler-service]] for the control-plane internals (HA, etcd,
persistence sizing).

## Job model

A job has:

| Field | Notes |
|---|---|
| `Name` | case-sensitive, **globally unique** across services on the runtime |
| `Schedule` | 6-field cron, Go duration string, period alias, or RFC3339 datetime |
| `DueTime` | optional one-shot or "not before" timestamp |
| `Ttl` | optional expiration after which the job is no longer triggered |
| `RepeatCount` | how many times to fire (omit for unlimited) |
| `Payload` | opaque bytes delivered to the trigger handler |
| `Overwrite` | replace existing job with same name (default: collision = error) |
| `FailurePolicy` | optional retry/drop behavior on app errors |

### Schedule syntax

- **Cron (6 fields)**: `seconds minutes hours day-of-month month day-of-week`.
  Resolution is seconds.
- **Period aliases**: `@every <duration>`, `@yearly`/`@annually`, `@monthly`,
  `@weekly`, `@daily`/`@midnight`, `@hourly`.
- **Duration strings**: Go `time.ParseDuration` syntax — `ns`, `us`, `ms`,
  `s`, `m`, `h`. Supports sub-second values like `500ms`.
- **RFC3339 timestamps**: time zone honored if included; otherwise the Dapr
  server's local TZ is used.

## Triggering

When a job fires, Scheduler invokes **only the app that scheduled it** (other
apps with the same `app-id` cannot intercept). Call shape depends on what the
app registered:

- **gRPC AppCallback** registered → Scheduler calls
  `OnJobEventAlpha1(JobEventRequest)`.
- **No gRPC handler** → Scheduler does
  `POST /job/<job-name>` with body fields `Schedule`, `RepeatCount`, `DueTime`,
  `Ttl`, `Payload`, `Overwrite`, `FailurePolicy`.

If the app has multiple replicas, Scheduler **load-balances randomly** across
them. Strict pod locality is not provided — use actor reminders with a
per-replica unique actor type if you need it.

## Failure semantics

- **At-least-once** delivery, never before due time, **no upper bound on
  lateness** if no app instance is reachable.
- Client-side errors (app returned non-success) are retried at **1s intervals,
  3 attempts** by default. Tunable via `FailurePolicy`.
- If no sidecar is reachable for the target app, jobs queue in Scheduler's
  **staging queue** until one comes up — pending jobs are not lost when an app
  is down.
- Jobs and actor reminders survive Scheduler restarts (etcd-persisted).
- Deleting a Kubernetes namespace deletes all jobs and reminders for that
  namespace.

## Lifecycle

```
schedule ──▶ [stored in etcd] ──▶ [due time reached]
                                       │
                                       ▼
                            [pick app replica]
                                       │
                          ┌────────────┴───────────┐
                          ▼                        ▼
                    gRPC OnJobEvent         POST /job/<name>
                          │                        │
                          ▼                        ▼
                    success ──▶ [keep / repeat / delete based on RepeatCount + Ttl]
                    error   ──▶ retry up to 3× @ 1s; then FailurePolicy
                    no app  ──▶ staging queue, redrive when sidecar appears
```

Inspect, list, delete via the **`dapr scheduler`** CLI (`list`, `get`,
`delete`, `delete-all`, `export`, `import`; add `-k` for Kubernetes).

## Code example

Go SDK:

```go
job := daprc.Job{
    Name:     "prod-db-backup",
    Schedule: "@every 1s",
    Repeats:  10,
    Data:     &anypb.Any{Value: jobData},
}
client.ScheduleJob(ctx, &job)
```

.NET SDK exposes `daprJobsClient.ScheduleJobAsync(name, DaprJobSchedule.FromDuration(...), payload, repeats)` and registers the trigger endpoint via `app.MapDaprScheduledJobHandler(...)`.

## When to reach for Jobs vs alternatives

| Need | Pick |
|---|---|
| One-shot timer, durable across restarts | Jobs |
| Recurring schedule (backup, ETL, report) | Jobs |
| Per-actor timer with locality + state | Actor reminders (also via Scheduler under the hood) |
| Long-running, multi-step orchestration | [[dapr-workflow]] |
| Queue-driven async work, no schedule | [[dapr-pubsub]] |

## Cross-references

- [[dapr-scheduler-service]] — control-plane that persists and triggers jobs
- [[dapr-workflow]] — workflows internally schedule activity reminders via
  the same Scheduler service
- [[dapr-pubsub]] — alternate for event-driven (not time-driven) async work

## Sources

- [Jobs overview](../sources/dapr-jobs-overview.md) — backs at-least-once
  guarantee, "scheduler not executor" framing, and use cases.
- [Jobs features and concepts](../sources/dapr-jobs-features.md) — backs
  schedule syntax, trigger callback contract, and `dapr scheduler` CLI.
- [How-to: schedule and handle jobs](../sources/dapr-jobs-howto.md) — backs
  the .NET and Go SDK code patterns.
- [Scheduler service](../sources/dapr-scheduler-service.md) — backs failure
  semantics, retries, staging queue, peer-not-leader architecture.
