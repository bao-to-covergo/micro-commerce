---
title: Dapr Secrets
type: concept
tags: [dapr, secrets, security, key-vault, scoping]
created: 2026-05-01
updated: 2026-05-01
sources:
  - sources/dapr-secrets-overview.md
  - sources/dapr-secrets-howto.md
  - sources/dapr-secrets-scopes.md
---

# Dapr Secrets

A uniform HTTP/gRPC API exposed by the Dapr sidecar that lets app code fetch
secrets from a configured backend (Vault, Azure Key Vault, AWS Secrets Manager,
GCP Secret Manager, Kubernetes Secrets, etc.) without importing any vendor
SDK. Apps talk only to Dapr; the actual store is described declaratively as
a `Component` of `type: secretstores.<provider>` and can be swapped per
environment without code changes.

## API shape

Two operations:

- **`getSecret(storeName, key)`** — returns a single secret as a
  `Map<string,string>` (multi-value secrets are first-class).
- **`getBulkSecret(storeName)`** — returns every secret in the store the
  caller has access to.

HTTP form:

```text
GET /v1.0/secrets/{storeName}/{key}
GET /v1.0/secrets/{storeName}/bulk
```

All major SDKs (.NET, Java, Python, Go, JavaScript) wrap both calls.

## Component pattern

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: localsecretstore
spec:
  type: secretstores.local.file
  version: v1
  metadata:
    - name: secretsFile
      value: secrets.json
    - name: nestedSeparator
      value: ":"
```

`secretstores.local.file` is **dev-only** — production should use a managed
backend. The `secretsFile` path is relative to the cwd of `dapr run`.

## Kubernetes built-in store

In Kubernetes, Dapr enables a built-in store named `kubernetes` automatically
(via Helm defaults or `dapr init -k`). Disable it per-pod with the deployment
annotation:

```yaml
annotations:
  dapr.io/disable-builtin-k8s-secret-store: "true"
```

Default is `false` (built-in store is on).

On AKS, Dapr can authenticate to Azure Key Vault using pod identities or
managed identities — no secret material on disk.

## Referencing secrets from other components

The recommended production pattern: keep credentials out of component YAML
and reference a secret store instead.

```yaml
spec:
  metadata:
    - name: connectionString
      secretKeyRef:
        name: mySecretName
        key: mySecretKey
auth:
  secretStore: localsecretstore
```

Any pub/sub, state, or binding component supports this — pair with a managed
secret store for rotation and access control.

## Scoping

Configured under `spec.secrets.scopes[*]` of a `Configuration` resource and
bound to a pod via the `dapr.io/config: appconfig` annotation. Three knobs
per store:

- `defaultAccess` — `allow` (implicit default) or `deny`
- `allowedSecrets` — explicit allow-list
- `deniedSecrets` — explicit deny-list

> "The `allowedSecrets` and `deniedSecrets` list values take priority over
> the `defaultAccess` policy."

Effective combinations:

| `defaultAccess` | `allowedSecrets` | `deniedSecrets` | Behavior |
|---|---|---|---|
| (omitted) | (empty) | (empty) | allow everything (default) |
| `deny` | `[s1, s2]` | (empty) | strict allow-list — only `s1`, `s2` |
| `allow` | (empty) | `[s1]` | blocklist — everything except `s1` |
| `allow` | `[s1, s2]` | (empty) | collapses to allow-list of just those keys |
| `deny` | (empty) | `[s1]` | deny-all (deny-list redundant) |

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: appconfig
spec:
  secrets:
    scopes:
      - storeName: vault
        defaultAccess: deny
        allowedSecrets: ["secret1", "secret2"]
```

Don't forget to also scope the built-in `kubernetes` store when locking down
on Kubernetes.

## Representative supported backends

- HashiCorp Vault
- Azure Key Vault
- AWS Secrets Manager / AWS Parameter Store
- GCP Secret Manager
- Kubernetes Secrets (built-in default in K8s mode)
- `local.file` / `local.env` (development only)

The full upstream list is longer; see Dapr's component reference for the
complete catalog (not yet ingested into this wiki).

## Cross-references

- [[dapr-pubsub]] — broker credentials reference secret stores via
  `secretKeyRef`
- [[dapr-runtime]] — the sidecar that exposes `/v1.0/secrets/...`

## Sources

- [Secrets overview](../sources/dapr-secrets-overview.md) — backs the
  Kubernetes built-in store annotation and AKS pod identity story.
- [How-to: secrets](../sources/dapr-secrets-howto.md) — backs the HTTP shape,
  SDK calls, and the local file component.
- [Secrets scoping](../sources/dapr-secrets-scopes.md) — backs the
  precedence table and the `Configuration`-binding pattern.
