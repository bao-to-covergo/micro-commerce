---
title: Dapr Workflow Management How-To (source)
type: source
tags: [dapr, workflow, howto, management]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/howto-manage-workflow.md
---

# Dapr Workflow Management How-To (source)

Operations cookbook: start, list, history, suspend/resume, terminate,
raise event, rerun, purge — via CLI, SDK, and HTTP. Documents the full
HTTP endpoint set and the purge restrictions.

## Key takeaways

- CLI commands accept `-k`/`--kubernetes` and `--namespace`.
- `dapr workflow rerun <id>` supports `--event-id` and `--new-instance-id`.
- Purging via CLI also deletes associated Scheduler reminders.
- A workflow client must be running for the same `app-id` during purge;
  `--force` bypasses but may corrupt state.
- `dapr scheduler list --filter workflow` manages workflow reminders;
  `delete`, `delete-all`, `export`/`import` for backup.
- HTTP base path: `/v1.0/workflows/dapr/<...>`. Component name is literally
  `dapr`.
- Instance IDs allow only alphanumerics, `_`, and `-`.
- Purge only allowed for `COMPLETED`, `FAILED`, `TERMINATED`.

## Verbatim quotes

- "It is required that a workflow client is running in the application to
  perform purge operations."
- "Only workflow instances in the COMPLETED, FAILED, or TERMINATED state
  can be purged."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/howto-manage-workflow.md`
