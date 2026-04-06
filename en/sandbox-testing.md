---
title: "Sandbox testing"
---

Sandbox companies are used for isolated Fintoro API testing. They do not use a separate host or base path.
## How the sandbox model works

- You can create multiple sandbox companies under one production company.
- Production and sandbox companies use the same base URL `https://app.fintoro.sk/api/public/v1`.
- The token decides which company you work with, not the host or path.
- A sandbox company has limited access to some Fintoro sections.
- For Fintoro API testing, the sandbox company acts as an isolated test environment and has its own tokens.

## Where to create a sandbox company

In Fintoro, open the production company and go to the integrations area for Fintoro API. This is where you manage tokens and sandbox companies. There you can create sandbox companies for a specific integration scenario, partner, or testing environment.

We recommend creating a separate sandbox company for each larger integration scenario or partner.

## Same host, different token

```bash
# Production company
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <production-company-token>'

# Sandbox company
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <sandbox-company-token>'
```

The URL is identical in both cases. Only the token changes.

## Recommended testing flow

1. Create a dedicated sandbox company for the integration scenario.
2. Create sandbox company tokens for the read-only or write flows you need.
3. Test lookups, read flows, and then write flows with `Idempotency-Key`.
4. Keep production and sandbox secrets, logs, and token naming separate.
5. Clean up the sandbox company or its tokens when the test cycle is finished.
