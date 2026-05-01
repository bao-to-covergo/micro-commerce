---
title: Dapr Secrets Scoping (source)
type: source
tags: [dapr, secrets, scopes, security]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/secrets/secrets-scopes.md
---

# Dapr Secrets Scoping (source)

Secret scoping policies are attached to a Dapr `Configuration` resource and
restrict which secrets an app can read. Three scenarios: deny all,
allow-list, deny-list, plus a precedence table.

## Key takeaways

- Scopes live under `spec.secrets.scopes` of a `Configuration` and bind to a
  pod via `dapr.io/config: appconfig`.
- Each scope keys off `storeName` with `defaultAccess` (`allow`/`deny`),
  `allowedSecrets`, `deniedSecrets`.
- `defaultAccess` defaults to `allow` when omitted.
- `allowedSecrets` / `deniedSecrets` always override `defaultAccess`.
- Applies uniformly to local, Kubernetes, and cloud secret stores.
- The default Kubernetes store is named `kubernetes` and should be locked
  down explicitly when denying all.

## Verbatim quotes

- "any secret defined within that store is accessible by default from the
  Dapr application."
- "The `allowedSecrets` and `deniedSecrets` list values take priority over
  the `defaultAccess` policy."
- Scenario row: `defaultAccess: deny`, `deniedSecrets: ["s1"]`,
  `allowedSecrets: empty` → "deny" (deny list redundant under default-deny).

## See also

- Concept: [[dapr-secrets]]
- Raw: `wiki/raw/developing-applications/building-blocks/secrets/secrets-scopes.md`
