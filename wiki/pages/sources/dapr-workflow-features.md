---
title: Dapr Workflow Features and Concepts (source)
type: source
tags: [dapr, workflow, determinism, replay]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-features-concepts.md
---

# Dapr Workflow Features and Concepts (source)

Concept reference covering identity, replay/event-sourcing, activities,
child workflows, timers, retry policies, external events, purge restrictions,
and the determinism rules that govern workflow code.

## Key takeaways

- Workflow state stored as event-sourced history; replay rehydrates from
  history.
- Each activity executes **at least once** — implement idempotently.
- Only one workflow instance per ID at a time; ID reusable after completion.
- `continue-as-new` truncates history and starts fresh with new history.
- Retry policy params: max attempts, first retry interval, backoff
  coefficient, max retry interval, retry timeout. Setting attempts to 0
  disables retries entirely.
- External events: name + payload, FIFO for same-name; events received
  before subscription are buffered.
- Purge only allowed in `COMPLETED`, `FAILED`, or `TERMINATED` state.
- Determinism rules: no `DateTime.UtcNow`, `Guid.NewGuid`, env vars, file
  I/O, HTTP, or background threads in workflow body. Use SDK context APIs.

## Verbatim quotes

- "executed at least once as part of a workflow's execution"
- "workflow code needs to be deterministic"
- "Don't declare JavaScript workflow as `async`. The Node.js runtime doesn't
  guarantee that asynchronous functions are deterministic."
- "Workflow retry policies are durable and maintain their state across
  application restarts, whereas Dapr Resiliency policies are not durable"

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-features-concepts.md`
