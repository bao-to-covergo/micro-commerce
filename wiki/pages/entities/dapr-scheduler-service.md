---
title: Dapr Scheduler Service
type: entity
tags: [dapr, scheduler, control-plane, etcd, jobs, reminders]
created: 2026-05-01
updated: 2026-05-01
sources:
  - sources/dapr-scheduler-service.md
  - sources/dapr-scheduler-kubernetes-persistence.md
  - sources/dapr-scheduler-self-hosted-persistence.md
  - sources/dapr-jobs-overview.md
---

# Dapr Scheduler Service

The Dapr control-plane process that persists scheduled work and triggers it at
the due time. Scheduler is the engine behind multiple user-facing features —
[[dapr-jobs]], actor reminders, [[dapr-workflow]] timers and activity
reminders all converge on the same service. Job records are stored in
embedded etcd (or external etcd) and triggers fire by calling back into the
originating app's sidecar.

## Architecture

> "There is no concept of a leader Scheduler instance. All Scheduler service
> replicas are considered peers."

Jobs are sharded across replicas for trigger load-balancing; a given job is
fired by **exactly one** replica. The peer model is at the Scheduler layer —
the embedded etcd inside each replica still uses Raft internally for its own
consensus (tunable via `--etcd-initial-election-tick-advance`), but that's
implementation detail.

Storage modes:

| Mode | Set via | Scaling |
|---|---|---|
| **Embedded etcd** (default) | (default) | Replica count cannot change without data loss |
| **External etcd** | `--etcd-embed=false` + `--etcd-client-endpoints=...` | Replicas scale freely; **triggering pauses during scale events** |

## What's inside Scheduler

Job IDs are namespaced by type prefix:

- `app/{app-id}/{job-name}` — Jobs API entries
- `actor/{actor-type}/{actor-id}/{reminder-name}` — actor reminders
- `activity/...` — workflow activity reminders
- `workflow/...` — workflow timers

Inspect via the **`dapr scheduler`** CLI (`list`, `get`, `delete`,
`delete-all`, `export`, `import`; add `-k` for Kubernetes).

## Triggering

When a job is due, Scheduler invokes only the originating app — multiple
apps with the same `app-id` cannot intercept each other's jobs. Among
replicas of the originating app, Scheduler picks **one randomly** (load
balancing). Strict locality is not provided; users that need it should use
actor reminders with a per-replica unique actor type.

Trigger callback shape:

- gRPC AppCallback registered → `OnJobEventAlpha1(JobEventRequest)`
- otherwise → `POST /job/<job-name>` with body fields `Schedule`,
  `RepeatCount`, `DueTime`, `Ttl`, `Payload`, `Overwrite`, `FailurePolicy`

## Failure semantics

> "When the Scheduler service triggers a job and it has a client side error,
> the job is retried by default with a 1s interval and 3 maximum retries."

> "For non-client side errors ... it is placed in a staging queue within the
> Scheduler service. Jobs remain in this queue until a suitable sidecar
> instance becomes available."

Net effect: an app being down does **not** drop pending triggers — they
reappear once a sidecar comes back. Jobs survive Scheduler restarts because
they're persisted in etcd.

## Self-hosted (Docker) persistence

Default volume name: `dapr_scheduler` on the Docker host. Survives restarts.
Override during init:

```bash
dapr init --scheduler-volume my-scheduler-volume
```

Switching volumes requires a full Dapr uninstall first (Scheduler container is
recreated). HA recommended for production. **Switching between non-HA and HA
modes requires removing the existing data dir — data loss.**

## Kubernetes persistence

A StatefulSet `dapr-scheduler-server` with PVCs
`dapr-scheduler-data-dir-dapr-scheduler-server-{0,1,2}` (HA = 3 replicas) on
the cluster's default StorageClass.

Default PVC size is **`1Gi` — too small for production**. Recommended ≥ 16Gi
for prod, with `dapr_scheduler.cluster.storageSize` and
`dapr_scheduler.etcdSpaceQuota` set in sync. A typical install:

```bash
dapr init -k \
  --set dapr_scheduler.cluster.storageSize=16Gi \
  --set dapr_scheduler.etcdSpaceQuota=16Gi
```

Symptom of overflow: `error running scheduler: etcdserver: mvcc: database
space exceeded`.

> "Etcd persists historical transactions and data in form of Write-Ahead Logs
> (WAL) and snapshots. This means the actual disk usage of Scheduler will be
> higher than the current observable database state, often by a number of
> multiples."

Resizing existing PVCs requires `allowVolumeExpansion: true` on the
StorageClass, deleting the StatefulSet with `--cascade=orphan`, editing PVCs,
then reinstalling. Custom StorageClass via
`dapr_scheduler.cluster.storageClassName=my-storage-class`.

For non-prod, non-HA only, ephemeral in-memory storage is available via
`dapr_scheduler.cluster.inMemoryStorage=true` — **data lost on restart**.

PVCs are **intentionally retained** across `dapr uninstall` (safety). Deleting
a Kubernetes namespace deletes all jobs and reminders for that namespace.

## Workflow load on Scheduler

> "Workflows create a large number of jobs as Actor Reminders, however these
> jobs are short lived — matching the lifecycle of each workflow execution."

If the deployment uses workflows heavily, expect higher Scheduler load and
size accordingly.

## Tunable etcd defaults

| Flag | Default | Notes |
|---|---|---|
| `--etcd-backend-batch-interval` | `50ms` | |
| `--etcd-backend-batch-limit` | `5000` | |
| `--etcd-compaction-mode` | `periodic` | |
| `--etcd-compaction-retention` | `10m` | |
| `--etcd-snapshot-count` | `10000` | |
| `--etcd-max-snapshots` | `10` | |
| `--etcd-max-wals` | `10` | |

## Disabling Scheduler

If you don't use Jobs, actor reminders, or workflows, disable the service to
save resources:

```yaml
global:
  scheduler:
    enabled: false
```

## Cross-references

- [[dapr-jobs]] — application-facing API on top of Scheduler
- [[dapr-workflow]] — uses Scheduler reminders for timers and activity
  retries
- [[dapr-actors]] — actor reminders also live in Scheduler

## Sources

- [Scheduler service](../sources/dapr-scheduler-service.md) — backs the
  peer-not-leader architecture, retry semantics, and tunable flags.
- [Kubernetes persistence](../sources/dapr-scheduler-kubernetes-persistence.md)
  — backs PVC sizing, the WAL-multiplier warning, and resize procedure.
- [Self-hosted persistence](../sources/dapr-scheduler-self-hosted-persistence.md)
  — backs the Docker volume name and `--scheduler-volume` override.
- [Jobs overview](../sources/dapr-jobs-overview.md) — backs the
  one-replica-fires-it scheduling guarantee and use cases.
