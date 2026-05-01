---
title: Dapr Workflow Patterns (source)
type: source
tags: [dapr, workflow, patterns, saga]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-patterns.md
---

# Dapr Workflow Patterns (source)

Catalogues the canonical workflow patterns: task chaining, fan-out/fan-in,
async HTTP API, monitor (eternal), external system interaction, and
compensation/saga.

## Key takeaways

- Task chaining: sequential `await`; runtime auto-saves and resumes from
  last completed step.
- Fan-out/fan-in: schedule N, `WhenAll`. Cap with `MaxParallelism`.
- Async HTTP API: POST to `/start?instanceID=...`, poll GET status.
  `runtimeStatus` enum: `RUNNING`, `COMPLETED`, `FAILURE`, `TERMINATED`.
- Monitor pattern: `CreateTimer` + `continue-as-new` instead of
  `while(true)`.
- External event timeout surfaces as `TaskCanceledException` (.NET/Java).
- Saga: track completed steps, run compensations in reverse order on
  failure — no extra framework dependency.

## Verbatim quotes

- "If a workflow starts 100 parallel task executions and only 40 complete
  before the process crashes, the workflow restarts itself automatically
  and only schedules the remaining 60 tasks."
- "Rather than writing infinite while-loops (which is an anti-pattern),
  Dapr Workflow exposes a continue-as-new API"

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-patterns.md`
