---
title: Dapr Workflow History Retention Policy (source)
type: source
tags: [dapr, workflow, retention, cleanup]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/workflow/workflow-history-retention-policy.md
---

# Dapr Workflow History Retention Policy (source)

TTL configuration for terminal-state workflow history, preventing unbounded
state-store growth. Per-terminal-state durations + an `anyTerminal` fallback.
Policy is **not retroactive** — existing terminal workflows must be cleaned
up via `dapr workflow purge`.

## Key takeaways

- Default retention is **indefinite**.
- Eligible states: `Completed`, `Failed`, `Terminated`.
- Durations are Go duration strings (e.g. `72h`, `30m`).
- Retroactive cleanup: `dapr workflow purge --all-older-than ...` with
  optional `--all-filter-status`.
- A workflow client must be running for the same `app-id` during purge;
  `--force` risks state-machine corruption.

## Verbatim quotes

- "When adding or changing a retention policy, the policy only applies to
  workflows that newly reach a configured terminal state after the policy is
  in effect. It does not retroactively clean up workflows that are already
  in a terminal state."

## See also

- Concept: [[dapr-workflow]]
- Raw: `wiki/raw/developing-applications/building-blocks/workflow/workflow-history-retention-policy.md`
