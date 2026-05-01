---
title: Dapr Workflow Concurrency (source)
type: source
tags: [dapr, workflow, concurrency]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-concurrency.md
---

# Dapr Workflow Concurrency (source)

Per-sidecar concurrency caps for workflow and activity invocations,
configured on a Dapr `Configuration` resource.

## Key takeaways

- `maxConcurrentWorkflowInvocations` and `maxConcurrentActivityInvocations`
  default to **infinite**.
- Limits apply across all workflow/activity definitions (no per-name
  granularity).
- Effective cluster limit = replicas × configured value.
- Useful to prevent resource exhaustion or to drain a backlog.

## Verbatim quotes

- "These limits are imposed on a per sidecar basis, meaning that if you have
  10 replicas of your workflow app, the effective limit is 10 times the
  configured value."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-concurrency.md`
