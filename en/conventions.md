# API conventions

This section summarizes the rules that repeat across Public API v1.

## Requests

- The Public API uses JSON request and response payloads.
- Business payloads in Public API v1 use camelCase field names.
- All requests are company-scoped through the bearer token.
- On create operations, we recommend using `Idempotency-Key`. It is not required, but it makes the integration safer, helps prevent duplicates during retries, and makes write flows more robust.
- You can send the optional `Accept-Language` header with any Public API request.

## Response localization

- `Accept-Language` is evaluated across the entire Public API.
- The header localizes fixed lookups and the same system-owned objects when they are returned as nested objects in list, detail, create, or update responses.
- Typical examples are countries, currencies, delivery methods, and similar system-owned lookups.
- User-generated data is not localized, for example client names, supplier names, warehouse names, or free-text notes.
- Validation errors use the same header. When a dedicated validation translation is not available for the requested language, validation errors fall back to English. Czech requests fall back to Slovak.
- If the header is missing or unsupported, the response is returned in the default language of the Fintoro Public API.
- The response returns `Content-Language` with the language selected for the request.

Supported request language tags:

| Language | Preferred tag | Validation error language |
| --- | --- | --- |
| Slovak | `sk` | Slovak |
| English | `en` | English |
| Czech | `cs` | Slovak |
| German | `de` | English |
| Estonian | `et` | English |
| Spanish | `es` | English |
| French | `fr` | English |
| Croatian | `hr` | English |
| Hungarian | `hu` | English |
| Italian | `it` | English |
| Lithuanian | `lt` | English |
| Dutch | `nl` | English |
| Polish | `pl` | English |
| Portuguese | `pt` | English |
| Romanian | `ro` | English |
| Serbian | `sr` | English |
| Russian | `ru` | English |
| Slovenian | `sl` | English |
| Ukrainian | `uk` | English |

## Response model

- List endpoints wrap their results in an object with `data` and, when applicable, additional metadata such as `paginator`.
- Detail and create/update endpoints return the business object directly without a wrapper.
- Store `X-Request-Id` from the response header for debugging.

## Change detection and synchronization

- If you need to react to create, update, or delete events and keep an external system continuously synchronized, implement webhooks.
- Polling is not the recommended mechanism for change detection. Use it only for the initial import, post-incident backfills, or reconciliation checks.
- The recommended model is: use the webhook as the trigger and then fetch the full resource detail through Public API.
- Webhook payloads intentionally do not return the entire resource snapshot. The source of truth for detail remains the API reference of the specific endpoint.
- For delete events, assume that the detail endpoint may no longer return the deleted resource.

## Lookups and reference tables

- Fixed lookup endpoints return the full dataset without filtering or pagination.
- Fixed lookup IDs are stable within this API version.
- For a quick human-readable overview of stable IDs, use the [reference tables](./reference-tables.md).

## Resource-specific notes

- In document create flows, some values may be resolved from the client, company settings, or system resolvers. Always verify the exact behavior in the schema of the specific endpoint.
- `contact-activity-logs` attachments use a two-step upload flow through a separate upload endpoint and follow-up upload tokens.
- Document resources expose dedicated PDF download endpoints or `pdfDownloadUrl` in the response.
- Most mutable business resources can also emit webhook events. The exact event catalog is documented in [Webhooks](./webhooks.md).

## Versioning and contract

- This documentation describes Public API v1 under `/api/public/v1`.
- If you need the exact payload shape, enums, or validation rules, the source of truth is the [API reference](./api-reference/index.mdx).
