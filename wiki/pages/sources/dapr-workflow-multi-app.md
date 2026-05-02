---
title: Dapr Multi-App Workflows (source)
type: source
tags: [dapr, workflow, multi-app, cross-service]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-multi-app.md
---

# Dapr Multi-App Workflows (source)

A workflow can route activities and child workflows to a different `appID`.
Parent owns history; final result returns to parent. Restrictions on
namespace, state store, and registration.

## Key takeaways

- Same namespace required.
- Same actor state store required.
- Missing target activity → parent retries indefinitely.
- Missing target app → retries per retry policy.
- SDK matrix: Go/Python/.NET full; Java activities only; JavaScript planned.
- Dapr 1.17 adds `WorkflowsRemoteActivityReminder` feature gate (off by
  default) — sends activity result via reminder if owning workflow app is
  offline.

## Verbatim quotes

- "Multi-application workflows require Dapr runtime v1.16.0 or later. .NET
  SDK support is available starting with v1.17.0."
- "all app IDs involved in a multi-application workflow must be in the same
  namespace"
- "the target app ID must have the activity or child workflow defined and
  registered, otherwise the parent workflow retries indefinitely"

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-multi-app.md`
