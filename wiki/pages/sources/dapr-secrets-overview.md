---
title: Dapr Secrets Overview (source)
type: source
tags: [dapr, secrets]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/secrets/secrets-overview.md
---

# Dapr Secrets Overview (source)

Why a secrets API exists (avoid vendor SDK boilerplate, multi-cloud), the
three usage steps, the Kubernetes built-in store enabled by default, and the
recommended pattern of referencing secrets in component metadata instead of
inlining credentials.

## Key takeaways

- Three steps: configure store component, retrieve via API, optionally
  reference secrets in component files.
- Kubernetes mode: built-in store named `kubernetes` enabled by default
  (Helm or `dapr init -k`).
- Per-pod opt-out via deployment annotation
  `dapr.io/disable-builtin-k8s-secret-store: "true"`.
- AKS supports pod identities / managed identities for Key Vault access.
- Best practice: reference secrets via `secretKeyRef` rather than inlining
  credentials, especially in production.
- Scoping is the access-control mechanism.

## Verbatim quotes

- "By default, Dapr enables a built-in Kubernetes secret store in
  Kubernetes mode, deployed via: The Helm defaults, or `dapr init -k`"
- "you can disable (not configure) the Dapr Kubernetes secret store by
  adding the annotation `dapr.io/disable-builtin-k8s-secret-store: \"true\"`
  to the deployment.yaml file. The default is `false`."

## See also

- Concept: [[dapr-secrets]]
- Raw: `wiki/raw/developing-applications/building-blocks/secrets/secrets-overview.md`
