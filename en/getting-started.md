---
title: "Getting started"
---

# Getting started

Use this guide to make the first authenticated request to Fintoro Public API. The goal is to verify the token and your orientation in the rest of the documentation without repeating every operational rule here.

## 1. Create a token or sandbox company

In Fintoro, open `Integrations > Public API`.

- In the production company, create tokens for real integration traffic.
- Under the production company, create multiple sandbox companies for isolated testing.
- Each company has its own tokens.

If you are testing in a sandbox, you do not use a different host. You still call `https://app.fintoro.sk/api/public/v1`, but with a sandbox company token.

## 2. Make your first request

For the first verification, use a simple read-only lookup endpoint:

```bash
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/countries \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept: application/json'
```

If the token is valid, you receive `200 OK` and a list of countries in `data`.

If the request returns `200 OK`, you have validated the host and token.

## 3. Prepare for write operations

1. Create a token with the lowest scope that still fits the use case.
2. Load the lookups and reference tables required for your payloads.
3. Verify read scenarios in the production or sandbox company.
4. If you need to react to changes continuously, implement [Webhooks](./webhooks.md). Use polling at most for the initial import or reconciliation checks.
5. Before the first write flow, review [API conventions](./conventions.md) and [Errors, rate limits, and retries](./errors-and-idempotency.md).

## 4. Review the basic decisions

- whether the integration belongs in the production company or a sandbox company,
- how you want to name tokens by system or partner,
- if you will consume webhooks, prepare the receiver endpoint, deduplication, and secret storage,
- which lookups you want to preload into cache or a local reference table,
- which tags in the API reference matter for your first write flow.

## Further reading

- [Authentication](./authentication.md)
- [Sandbox testing](./sandbox-testing.md)
- [Webhooks](./webhooks.md)
- [API conventions](./conventions.md)
- [Errors, rate limits, and retries](./errors-and-idempotency.md)
- [API reference](./api-reference/index.mdx)
