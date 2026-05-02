---
title: Dapr Jobs Features and Concepts (source)
type: source
tags: [dapr, jobs, cron, schedule]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/jobs/jobs-features-concepts.md
---

# Dapr Jobs Features and Concepts (source)

Deep dive on job identity, scheduling syntax (Cron / duration / period
aliases / RFC3339), trigger callback contract (gRPC + HTTP), and the
`dapr scheduler` CLI for managing jobs at runtime.

## Key takeaways

- Job names are case-sensitive and globally unique across services.
- 6-field cron: `seconds minutes hours day-of-month month day-of-week`.
- Period aliases: `@every`, `@yearly`/`@annually`, `@monthly`, `@weekly`,
  `@daily`/`@midnight`, `@hourly`.
- Duration strings follow Go `time.ParseDuration` (`ns`, `us`, `ms`, `s`,
  `m`, `h`); sub-second precision for durations.
- RFC3339 timestamps may include time zone; otherwise the Dapr server's
  local TZ is used.
- gRPC trigger via `OnJobEventAlpha1`; HTTP trigger via `POST /job/<job-name>`
  with body fields `Schedule`, `RepeatCount`, `DueTime`, `Ttl`, `Payload`,
  `Overwrite`, `FailurePolicy`.
- `dapr scheduler` CLI: `list`, `get`, `delete`, `delete-all`, `export`,
  `import`.

## Verbatim quotes

- "Supports sub-second precision when using duration values (for example
  `500ms`). Actual trigger resolution may vary by runtime; Cron-based
  schedules are at the seconds level only."
- "if a timestamp is provided with a time zone via the RFC3339 specification,
  that time zone is used. When not provided, the time zone used by the
  server running Dapr is used."
- "If a gRPC server isn't registered with Dapr when the application starts
  up, Dapr instead triggers jobs by making a POST request to the endpoint
  `/job/<job-name>`."

## See also

- Concept: [[dapr-jobs]]
- Raw: `wiki/raw/developing-applications/building-blocks/jobs/jobs-features-concepts.md`
