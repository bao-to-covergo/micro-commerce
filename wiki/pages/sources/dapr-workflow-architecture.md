---
title: Dapr Workflow Architecture (source)
type: source
tags: [dapr, workflow, actors, internals]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-architecture.md
---

# Dapr Workflow Architecture (source)

Documents the internal architecture: built on Dapr actors via `durabletask-go`,
two actor types per app (`workflow` and `activity`), reminders for
fault-tolerance, and explicit state-store usage.

## Key takeaways

- Actor types per app: `dapr.internal.{namespace}.{appID}.workflow` and
  `...activity`.
- Activity actor IDs: `{workflowID}::{seq}::{generation}`.
- State keys: `inbox-NNNNNN`, `history-NNNNNN`, `customStatus`, `metadata`.
- Reminders are one-shot — expire after success; on crash, reactivate to
  retry "forever".
- Records-per-task: start=5, activity=3, timer=3, raise-event=3,
  child-workflow=8.
- Supported state stores: PostgreSQL, MySQL, SQL Server, SQLite, Oracle,
  CockroachDB, MongoDB, Redis.
- Cosmos DB caps items at 2 MB UTF-8 JSON.
- Third actor type `dapr.internal.{ns}.{appID}.executor` exists under
  Workflows Clustered Deployment preview (Dapr Shared).

## Verbatim quotes

- "Workflow activity code currently does not have access to the trace
  context."
- "By default, there are no global limits imposed on workflow and activity
  concurrency."
- "The Dapr Workflow engine requires that all instances of a workflow app
  register the exact same set of workflows and activities."
- "Workflow actor state remains in the state store even after a workflow has
  completed. Creating a large number of workflows could result in unbounded
  storage usage."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-architecture.md`
