---
title: Dapr Secrets How-To (source)
type: source
tags: [dapr, secrets, howto]
created: 2026-05-01
updated: 2026-05-01
raw: ../../raw/developing-applications/building-blocks/secrets/howto-secrets.md
---

# Dapr Secrets How-To (source)

Walkthrough configuring a `secretstores.local.file` component and retrieving
secrets via HTTP curl and SDKs (.NET, Java, Python, Go, JS). Includes
both `getSecret` and `getBulkSecret` examples.

## Key takeaways

- Local file secret store is dev-only; production should use a managed
  backend.
- `secretsFile` path is relative to the cwd of `dapr run`.
- `nestedSeparator` metadata controls flattening of nested JSON keys (the
  example uses `:`).
- HTTP shape: `GET http://localhost:<dapr-http-port>/v1.0/secrets/<store>/<key>`.
- SDKs expose both `GetSecret`/`get_secret` and `GetBulkSecret`/`get_bulk_secret`.
- Component `type: secretstores.local.file`, `version: v1`.

## Verbatim quotes

- "In a production-grade application, local secret stores are not
  recommended."
- "The path to the secret store JSON is relative to where you call
  `dapr run`."

## See also

- Concept: [[dapr-secrets]]
- Raw: `wiki/raw/developing-applications/building-blocks/secrets/howto-secrets.md`
