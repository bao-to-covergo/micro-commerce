---
title: Dapr Workflow Authoring How-To (source)
type: source
tags: [dapr, workflow, howto, sdk]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/howto-author-workflow.md
---

# Dapr Workflow Authoring How-To (source)

Step-by-step authoring guide for all five SDKs: write activities, write
workflow, register in runtime, run via Dapr CLI. Emphasizes that I/O belongs
in activities, not the orchestrator body.

## Key takeaways

- Sidecar drives execution; activities live in app code.
- Go: `workflow.NewRegistry()` + `AddWorkflow`/`AddActivity`; client via
  `client.NewWorkflowClient()` and `StartWorker(ctx, r)`.
- .NET: `AddDaprWorkflow(options => { options.RegisterWorkflow<...>();
  options.RegisterActivity<...>(); })`.
- Java: `WorkflowRuntimeBuilder().registerWorkflow(...).registerActivity(...)`.
- Diagrid Dashboard mentioned for visualization:
  `docker run -p 8080:8080 ghcr.io/diagridio/diagrid-dashboard:latest`.
- Go SDK supports: `CallActivity`, `WaitForExternalEvent`, `CreateTimer`.

## Verbatim quotes

- "The Dapr sidecar doesn't load any workflow definitions."
- "Because of how replay-based workflows execute, you'll write logic that
  does things like I/O and interacting with systems inside activities.
  Meanwhile, the workflow method is just for orchestrating those activities."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/howto-author-workflow.md`
