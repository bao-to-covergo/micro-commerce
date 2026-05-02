---
title: Dapr Workflow Versioning (source)
type: source
tags: [dapr, workflow, versioning, rollouts]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-versioning.md
---

# Dapr Workflow Versioning (source)

Two complementary strategies — patch identifiers (`IsPatched`) for in-place
tweaks, and named workflow versioning (`WorkflowV1`/`V2`) for clean breaks.
Use them in conjunction. Mismatched versions during rollout produce a
`Stalled` state.

## Key takeaways

- Patch identifiers must be unique per workflow; never reused across
  deployments. Identifier list lives in workflow history → larger history.
- Patches require the original code to remain unchanged.
- Stall causes: identifier reuse, removal/rename, change of patch order.
- Named versioning: register `WorkflowV2` with `isLatest=true`. Existing
  instances stay on their original version — **no auto-migration**.
- Stall reason example: `VERSION_NAME_MISMATCH;description=Version not
  available: workflow_v1`.

## Verbatim quotes

- "Workflows are required to be deterministic meaning that every time they
  are run, they produce precisely the same output as the last time."
- "the runtime does not migrate workflows between versions sequentially"
- "the original code must remain available and unchanged when applying a
  patch"

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-versioning.md`
