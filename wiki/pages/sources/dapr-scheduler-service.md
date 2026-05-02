---
title: Dapr Scheduler Service (source)
type: source
tags: [dapr, scheduler, control-plane, etcd]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/concepts/dapr-services/scheduler.md
---

# Dapr Scheduler Service (source)

Authoritative description of the Scheduler control-plane service: peer
architecture (no leader), embedded or external etcd, locality, retries with
staging queue for unreachable sidecars, HA semantics, CLI, and tunable etcd
flags.

## Key takeaways

- All Scheduler replicas are peers; jobs load-balanced across them.
- Default trigger locality: random load-balance across an app's replicas.
- Strict locality requires actor reminders + per-replica unique actor type.
- Client-side errors retry at 1s/3 attempts default; non-client-side errors
  go to a staging queue until a sidecar is reachable.
- Self-hosted: launched by `dapr init` (Docker) or runnable as a process in
  slim mode. HA recommended in prod. Switching HA modes requires deleting
  the data dir (data loss).
- Kubernetes: always HA. Cannot scale replicas without data loss (embedded
  etcd). Deleting a namespace deletes all that namespace's jobs and
  reminders.
- Job ID prefixes: `app/{app-id}/{job-name}`,
  `actor/{actor-type}/{actor-id}/{reminder-name}`, `activity/...`,
  `workflow/...`.
- External etcd via `--etcd-embed=false` + `--etcd-client-endpoints`;
  triggering pauses during scale events.
- Disable Scheduler via `global.scheduler.enabled=false` if Jobs/Reminders/
  Workflows unused.

## Verbatim quotes

- "There is no concept of a leader Scheduler instance. All Scheduler
  service replicas are considered peers."
- "When the Scheduler service triggers a job and it has a client side
  error, the job is retried by default with a 1s interval and 3 maximum
  retries."
- "For non-client side errors ... it is placed in a staging queue within
  the Scheduler service. Jobs remain in this queue until a suitable sidecar
  instance becomes available."
- "Scheduler always runs in high availability (HA) mode in Kubernetes
  deployments. Scaling the Scheduler service replicas up or down is not
  possible without incurring data loss due to the nature of the embedded
  data store."
- "If switching between non-HA and HA modes, the existing data directory
  must be removed, which results in loss of jobs and actor reminders."
- "during scale events [of external-etcd Scheduler], job triggering will
  be paused."

## Tunable etcd defaults

| Flag | Default |
|---|---|
| `--etcd-backend-batch-interval` | `50ms` |
| `--etcd-backend-batch-limit` | `5000` |
| `--etcd-compaction-mode` | `periodic` |
| `--etcd-compaction-retention` | `10m` |
| `--etcd-snapshot-count` | `10000` |
| `--etcd-max-snapshots` | `10` |
| `--etcd-max-wals` | `10` |

## See also

- Entity: [[dapr-scheduler-service]]
- Concept: [[dapr-jobs]]
- Raw: `wiki/raw/concepts/dapr-services/scheduler.md`
