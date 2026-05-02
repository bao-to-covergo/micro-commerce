---
title: Persisting Scheduler Data on Kubernetes (source)
type: source
tags: [dapr, scheduler, kubernetes, pvc, etcd]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/operations/hosting/kubernetes/kubernetes-persisting-scheduler.md
---

# Persisting Scheduler Data on Kubernetes (source)

How to configure persistent storage for Scheduler on Kubernetes: PVC sizing,
StorageClass selection, and resize procedure. Default 1Gi is too small for
production.

## Key takeaways

- Default PVC size `1Gi` against the cluster's default StorageClass.
- Recommended ≥ 16Gi for production.
- Overflow symptom: `error running scheduler: etcdserver: mvcc: database
  space exceeded`.
- Set on fresh install with `dapr_scheduler.cluster.storageSize=16Gi` and
  `dapr_scheduler.etcdSpaceQuota=16Gi`.
- To resize PVCs: ensure `allowVolumeExpansion: true`, delete StatefulSet
  with `--cascade=orphan`, edit PVCs, reinstall.
- PVC name pattern: `dapr-scheduler-data-dir-dapr-scheduler-server-{0,1,2}`.
- PVCs intentionally **not** deleted on `dapr uninstall`.
- Non-prod, non-HA only: `dapr_scheduler.cluster.inMemoryStorage=true`
  (data lost on restart).
- Custom StorageClass via `dapr_scheduler.cluster.storageClassName=...`.

## Verbatim quotes

- "the Scheduler service database embeds Etcd and writes data to a
  Persistent Volume Claim volume of size `1Gb`, using the cluster's default
  storage class."
- "Workflows create a large number of jobs as Actor Reminders, however
  these jobs are short lived — matching the lifecycle of each workflow
  execution."
- "Etcd persists historical transactions and data in form of Write-Ahead
  Logs (WAL) and snapshots. This means the actual disk usage of Scheduler
  will be higher than the current observable database state, often by a
  number of multiples."
- "When running in non-HA mode, the Scheduler can be optionally made to use
  ephemeral storage, which is in-memory storage that is **not** resilient
  to restarts."

## See also

- Entity: [[dapr-scheduler-service]]
- Raw: `wiki/raw/operations/hosting/kubernetes/kubernetes-persisting-scheduler.md`
