---
title: YARP Gateway
type: entity
tags: [gateway, yarp, reverse-proxy]
created: 2026-05-01
updated: 2026-05-01
sources: [../sources/microcommerce-claude-md.md]
---

`MicroCommerce.Gateway` is a YARP reverse proxy that sits between the Next.js
frontend and the [[api-service]]. It centralizes cross-cutting HTTP concerns so
the backend slices stay focused on domain logic.

## Responsibilities

- **CORS** — single allow-list for the storefront and admin origins
- **Rate limiting** — sliding-window policies applied per route
- **Auth** — JWT validation against Keycloak before traffic reaches the backend
- **Request correlation** — injects `X-Request-ID` for distributed tracing

The frontend always routes through the gateway; it never calls `ApiService`
directly. Aspire injects the gateway URL into the frontend as
`services__gateway__https__0`, exposed to the browser via `/api/config`.

## Cross-references

- [[microcommerce-platform]]
- [[api-service]]

## Sources

- [microcommerce-claude-md](../sources/microcommerce-claude-md.md)
