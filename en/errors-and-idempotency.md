---
title: "Errors, rate limits, and retries"
sidebarTitle: "Errors and idempotency"
---

This page explains which error states the Public API can return, how rate limiting works, and when retries are safe. It also covers `Idempotency-Key`, `X-Request-Id`, and webhook retry and deduplication behavior.
## HTTP errors and error responses

- `401` or another auth failure means the request does not have a usable bearer token. Typical causes are a missing, invalid, or revoked token.
- `403` means the token or company does not have permission for the operation.
- `404` means the resource does not exist in the current company context.
- `429` means the company exceeded the Public API rate limit.
- `422` means the request payload or query parameters failed validation.
- `500` means an unexpected internal error.
- `503` means a temporary service or dependency outage.

Always verify the exact error shape and available statuses on the specific endpoint in the API reference.

Error responses for all endpoints under `/api/public/*` are returned as JSON even when the request omits `Accept: application/json`. This also applies to route-level `404` responses and unexpected internal errors. Successful download endpoints that return PDF or other binary content are not affected and continue to return their original `Content-Type`.

## Rate limiting

- Public API uses a default limit of `120 requests per minute per company`.
- The limit is evaluated at the company level, not per token. Multiple tokens issued for the same company therefore share one rate-limit budget.
- All throttled endpoints return the `X-RateLimit-Limit` and `X-RateLimit-Remaining` headers even on successful responses.
- When the limit is exceeded, the API returns `429 Too Many Requests`; that response includes the same `X-RateLimit-Limit` and `X-RateLimit-Remaining` headers and additionally adds `Retry-After` and `X-RateLimit-Reset`.
- If your integration needs a higher limit, we also offer custom enterprise terms and limits. Contact us at [info@fintoro.sk](mailto:info@fintoro.sk).

## Idempotency-Key and safe retries

On create operations, we recommend sending `Idempotency-Key` when the endpoint supports it. If you repeat the same create request with the same key and the same payload, the backend returns the original result instead of creating a duplicate.

```bash
curl --request POST \
  --url https://app.fintoro.sk/api/public/v1/clients \
  --header 'Authorization: Bearer <company-token>' \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'Idempotency-Key: client-import-2026-03-17-001' \
  --data '{
    "name": "Acme s.r.o.",
    "email": "api@example.com"
  }'
```

If you reuse the same `Idempotency-Key` with a different payload, the endpoint rejects the request. Check the exact status code and error payload in the documentation for that endpoint.

Without `Idempotency-Key`, automatic retries of write requests are risky because a timeout or network failure may still happen after the backend already committed the write. Treat retries after `500` and `503` as safe only when you use `Idempotency-Key` and want to avoid duplicate writes.

## X-Request-Id and diagnostics

Fintoro returns `X-Request-Id`, which you should store in your integration logs. When investigating an incident, support case, or audit trail, this identifier makes request lookup and correlation with our internal logs significantly faster.

## Webhook retries and deduplication

If you use webhooks, assume an at-least-once delivery model:

- a non-`2xx` response, timeout, or network failure triggers a retry,
- the same `webhook-id` can arrive more than once,
- the receiver must deduplicate and safely process a redelivered event.

For a webhook receiver, we recommend:

- persisting `webhook-id` before downstream processing,
- returning `2xx` only after the request has been accepted and your own internal job has been queued,
- returning non-`2xx` only when you want Fintoro to retry the delivery.

## Implementation checklist

- generate the `Idempotency-Key` so it stays stable for one business attempt to create a resource,
- do not mix production and sandbox tokens or their request logs,
- use the validation response details to fix the request before sending it again,
- respect `Retry-After` and the rate-limit headers on `429` responses instead of aggressive retry loops,
- store `X-Request-Id` for support, audits, and incident handling,
- make your webhook receiver idempotent and deduplicate by `webhook-id`.
