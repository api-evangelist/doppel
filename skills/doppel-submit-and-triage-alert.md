---
name: Submit and triage a Doppel Brand Protection alert
description: Create an alert for a suspicious URL or phone number, poll its triage status, then tag it or request a takedown.
api: openapi/doppel-openapi-original.yml
operations: [create_alert, get-alert, update_alert]
---

# Submit and triage a Doppel Brand Protection alert

Use the Doppel V1 API (`https://api.doppel.com/v1`) to raise and work an alert on a suspicious entity.

## Auth
Send both headers on every request (recommended user-specific mode):
`x-api-key: <ORG_API_KEY>` and `x-user-api-key: <USER_API_KEY>` (keys from Doppel Vision API settings).

## Steps
1. **Create the alert** — `create_alert` (`POST /alert`). Body includes `entity` (a valid URL or phone number) and optional `brand`, `tags`, `source`, and up to 10 base64 `files`.
   - If an alert already exists for the entity you get HTTP 200 with the existing alert; a different brand for that entity returns 409.
   - 400 if `entity` is missing/invalid, is a protected asset (e.g. google.com), `tags` is malformed, `source` is unknown, or files violate the limits.
   - The alert is automatically pushed through Doppel's triage workflow after creation.
2. **Check status** — `get-alert` (`GET /alert`) with exactly one of `id` or `entity`. Returns the full alert including audit logs, tags, entity content, an AI alert summary, and a signed screenshot URL (expires in 1 hour). Poll until triage completes.
3. **Act on it** — `update_alert` (`PUT /alert`) with exactly one of `id` or `entity` and at least one field to change. Paired params must travel together: `tag_action`+`tag_name`, and `file_action`+`files`. Use this to tag, attach evidence, or drive a takedown/enforcement change.

## Conventions & errors
- Errors return `application/json` `{ "message": "..." }` (see `errors/doppel-problem-types.yml`).
- 429 means the per-operation quota was hit — back off and retry.
- No client idempotency key; re-`POST`ing the same `entity` returns the existing alert rather than a duplicate.
