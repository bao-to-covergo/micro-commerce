---
title: Dapr Jobs How-To: Schedule and Handle Triggered Jobs (source)
type: source
tags: [dapr, jobs, howto, sdk]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/jobs/howto-schedule-and-handle-triggered-jobs.md
---

# Dapr Jobs How-To (source)

Walkthrough using .NET and Go SDKs to register a trigger handler and
schedule a recurring `prod-db-backup` job at `@every 1s`. Clarifies that
only the service that scheduled a job receives its triggers.

## Key takeaways

- .NET: `AddDaprJobsClient()` + `app.MapDaprScheduledJobHandler(...)`
  registers `/job/<job-name>`.
- .NET schedules via `daprJobsClient.ScheduleJobAsync(name,
  DaprJobSchedule.FromDuration(...), payload, repeats)`.
- Go: `server.AddJobEventHandler("prod-db-backup", handler)` +
  `daprc.Job{ Name, Schedule: "@every 1s", Repeats: 10, Data }`.
- Sidecar must be started with `--app-protocol grpc` for the Go example.
- Payload is serialized bytes (UTF-8 JSON in .NET sample, `anypb.Any` in Go).

## Verbatim quotes

- ".NET ... only the service that schedules a job receives trigger
  invocations for it."

## See also

- Concept: [[dapr-jobs]]
- Raw: `wiki/raw/developing-applications/building-blocks/jobs/howto-schedule-and-handle-triggered-jobs.md`
