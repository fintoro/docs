---
title: "Fintoro API"
sidebarTitle: "Introduction"
description: "REST API for integrating Fintoro invoicing, CRM, and warehouse workflows into third-party systems."
---

Fintoro API is a REST API for integrating Fintoro invoicing, CRM, and warehouse workflows into third-party systems. It is intended for partners, internal integration teams, and customer implementations that need reliable read and write access to Fintoro data and also want to react to changes through outbound webhooks.

Use it to work with clients, suppliers, CRM, warehouses, documents, webhook subscriptions, and lookup data within one production or sandbox company context.

Production and sandbox companies use the same base URL `https://app.fintoro.sk/api/public/v1`. The company context is not determined by a different host or path, but by the bearer token issued for the target company.

## What this documentation includes

- onboarding guides for initial setup and operation,
- a webhook guide covering signing, delivery payloads, and the event catalog,
- the OpenAPI reference for the exact request and response contract,
- reference tables for fixed lookups with stable IDs,
- guidance for authentication, sandbox companies, idempotency, and request tracing.

If your integration needs to react to changes continuously and keep a downstream system synchronized with minimal delay, implement [Webhooks](/en/webhooks). That is what they are for in the Fintoro API. Use read endpoints for initial import, detail fetches, and reconciliation, not as the primary change-detection mechanism.

## Base URL

```plaintext
https://app.fintoro.sk/api/public/v1
```

## Getting started

For the first connection, continue to [Getting started](/en/getting-started), which covers token or sandbox setup, the first authenticated request, and the recommended next steps for write flows and webhooks.

## Production and sandbox companies

- Production and sandbox companies use the same host and the same base path.
- You can create multiple sandbox companies under one production company.
- Each sandbox company serves as an isolated environment for API testing.
- A sandbox company has limited access to some Fintoro sections, but for Fintoro API testing it works through its own tokens.
- For API requests, the host and base path stay the same. Only the token changes.

## Further reading

- [Getting started](/en/getting-started)
- [Authentication](/en/authentication)
- [Sandbox testing](/en/sandbox-testing)
- [API conventions](/en/conventions)
- [Errors and idempotency](/en/errors-and-idempotency)
- [Webhooks](/en/webhooks)
- [Reference tables](/en/reference-tables)
- [API reference](/en/api-reference)
