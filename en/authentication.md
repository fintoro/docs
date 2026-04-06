# Authentication

Fintoro Public API uses bearer tokens. A token is always issued for one specific company.

## Authorization header

Send every request with the `Authorization: Bearer <company-token>` header.

```bash
curl --request GET \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept-Language: en' \
  --header 'Accept: application/json'
```

`Accept-Language` is optional for any Public API request. It localizes system-owned labels in fixed lookups and in the same nested lookup objects returned by business responses, and it also affects validation errors. Supported tags and fallback rules are listed in the [API conventions](./conventions.md). It does not translate user-generated data.

## Scope model

- `read` is intended for syncs, reporting, BI, and other read-only scenarios.
- `write` allows both reading and writing, including create, update, and delete operations where the endpoint supports them.
- `write` is also required for webhook subscription management, including create, update, delete, and rotate-secret operations.

Use the lowest scope required by the integration.

## Company context of the token

- A token is always issued for one specific company.
- Production and sandbox companies use the same base URL.
- If you want isolated testing, use a sandbox company token.
- One production company can have multiple sandbox companies, and each sandbox company has its own tokens.

## Practical security rules

- Store the token securely when it is created. The plaintext value is shown during generation.
- A webhook secret is not a bearer token. Store it separately and never use it to authorize Public API requests.
- Avoid sharing one token across unrelated integrations when you can separate them.
- Name tokens by system or partner so they are easy to audit.
- Revoke unused or compromised tokens. A revoked token stops working in the Public API immediately and subsequent requests fail with `401 Unauthenticated.`, while the token record and its audit trail remain available in Fintoro.

## Where token management lives

Manage tokens in Fintoro under `Integrations > Public API`. In the same production-company screen you can also create sandbox companies and then manage sandbox company tokens for them.
