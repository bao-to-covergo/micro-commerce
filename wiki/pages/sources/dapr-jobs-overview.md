---
title: Dapr Jobs Overview (source)
type: source
tags: [dapr, jobs, scheduler]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/jobs/jobs-overview.md
---

# Dapr Jobs Overview (source)

Introduces Jobs as a scheduler-not-executor backed by the Dapr Scheduler
control-plane service with embedded etcd. Lists scenarios (delayed pub/sub,
scheduled service invocation, backups, ETL, reports, batch finance jobs) and
the at-least-once / never-before-due-time guarantees.

## Key takeaways

- Jobs in Dapr = Jobs API + Scheduler control-plane service.
- Scheduler also powers actor reminders internally.
- Job and user data persist in embedded etcd inside Scheduler.
- Default behavior: creating a job with an existing name fails unless
  `overwrite=true`; only the most recent job per name is kept (count resets).
- Multiple replicas can schedule the same job; only one Scheduler instance
  triggers it.

## Verbatim quotes

- "The jobs API is a job scheduler, not the executor which runs the job. The
  design guarantees *at least once* job execution with a bias towards
  durability and horizontal scaling over precision."
- "Guaranteed: A job is never invoked *before* the schedule time is due."
- "Not guaranteed: A ceiling time on when the job is invoked *after* the due
  time is reached."
- "All job details and user-associated data for scheduled jobs are stored in
  an embedded Etcd database in the Scheduler service."
- "The Scheduler service enables the scheduling of jobs to scale across
  multiple replicas, while guaranteeing that a job is only triggered by 1
  Scheduler service instance."

## See also

- Concept: [[dapr-jobs]]
- Entity: [[dapr-scheduler-service]]
- Raw: `wiki/raw/developing-applications/building-blocks/jobs/jobs-overview.md`
