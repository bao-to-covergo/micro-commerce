---
title: Persisting Scheduler Data Self-Hosted (source)
type: source
tags: [dapr, scheduler, self-hosted, docker]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/operations/hosting/self-hosted/self-hosted-persisting-scheduler.md
---

# Persisting Scheduler Data Self-Hosted (source)

In self-hosted (Docker) mode, Scheduler persists to a local Docker volume
`dapr_scheduler` that survives restarts. Override during init.

## Key takeaways

- Default volume: `dapr_scheduler`.
- On Docker Desktop the volume lives in the Docker Desktop VM filesystem.
- Override with `dapr init --scheduler-volume my-scheduler-volume`.
- Switching volumes requires a full Dapr uninstall first.

## Verbatim quotes

- "By default, the Scheduler service database writes this data to the local
  volume `dapr_scheduler`, meaning that **this data is persisted across
  restarts**."

## See also

- Entity: [[dapr-scheduler-service]]
- Raw: `wiki/raw/operations/hosting/self-hosted/self-hosted-persisting-scheduler.md`
