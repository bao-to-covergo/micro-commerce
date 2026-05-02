---
title: Dapr Workflow Overview (source)
type: source
tags: [dapr, workflow]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-overview.md
---

# Dapr Workflow Overview (source)

High-level intro to Dapr Workflow: built-in runtime + SDKs (Python, JS, .NET,
Java, Go), HTTP/gRPC management surface (start, query, pause/resume,
raise event, terminate, purge), child workflows with independent state and
auto-retry, and durable timers backed by reminders.

## Key takeaways

- Stateful, fault-tolerant orchestration; runs inside the sidecar.
- Child workflows are independent (own ID/history/status) but inherit the
  parent's lifecycle (terminate parent → terminate children).
- Cosmos DB and DynamoDB have payload/complexity limits.
- CLI lifecycle: `dapr workflow run|list|suspend|purge ...`.

## Verbatim quotes

- "Since Dapr workflows are stateful, they support long-running and
  fault-tolerant applications."
- "The workflow logic lives in your application and is orchestrated by the
  Dapr Workflow engine running in the Dapr sidecar via a gRPC stream."
- "terminating the parent workflow terminates all of the child workflows."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-overview.md`
