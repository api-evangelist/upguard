---
name: Wire platform notifications to a webhook
description: >-
  Discover UpGuard notification types, create an HMAC-signed webhook, verify
  deliveries with sample payloads, and manage the subscription.
api: openapi/upguard-cyberrisk-openapi-original.json
operations: [webhooks_notification_types, sample_webhook, create_webhook, list_webhooks, delete_webhook]
generated: '2026-07-21'
method: generated
---

# Wire platform notifications to a webhook

Authenticate with the `Authorization` API-key header; base URL
`https://cyber-risk.upguard.com/api/public`.

1. **Discover notification types** — `webhooks_notification_types`
   (`GET /webhooks/notification_types`) returns the catalog of subscribable
   events with ids and descriptions (e.g. "When a new data leak is
   published").
2. **Preview payloads** — `sample_webhook` (`GET /webhooks/sample`) returns
   example delivery data per notification type; deliveries carry a
   `description` plus a `context` metadata object.
3. **Create the webhook** — `create_webhook` (`POST /webhooks`) with `name`,
   `hook_url`, and `notification_type_ids[]` (use `webhook_type=workflow_webhook`
   only for workflow integrations). **Store the returned `signing_secret`
   immediately** — the 64-char hex HMAC secret is returned only once, at
   creation, and authenticates every delivery to your endpoint.
4. **Verify deliveries** — validate the HMAC signature on each incoming
   request against the stored signing secret before trusting the payload.
5. **Manage** — `list_webhooks` (`GET /webhooks`) to audit subscriptions;
   `delete_webhook` (`DELETE /webhooks`) to remove one.

Creation is not idempotent — check `list_webhooks` before re-creating after a
timeout. Errors use the `{ "error": "..." }` envelope; `429` signals
throttling.
