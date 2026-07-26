---
name: Pull Doppel Human Risk Management campaign results
description: List phishing-simulation campaigns, fetch one, and read its user engagement events.
api: openapi/doppel-openapi-original.yml
operations: [list-hrm-campaigns, get-hrm-campaign, list-hrm-events]
---

# Pull Doppel Human Risk Management (HRM) campaign results

## Auth
`x-api-key` + `x-user-api-key` headers.

## Steps
1. **List campaigns** — `list-hrm-campaigns` (`GET /hrm/campaigns`). Filter by status, campaign type, date range, and test flag; paginated.
2. **Get one campaign** — `get-hrm-campaign` (`GET /hrm/campaigns/{campaign_id}`). 404 if unknown.
3. **Read engagement events** — `list-hrm-events` (`GET /hrm/events`). Events track opens, link clicks, data submissions, reports, and voice-call outcomes, with joined user context (name, department, email). Filter by campaign, action type, user attributes, and date range; paginated.

## Real-time alternative
Instead of polling, subscribe to the `hrm_event` webhook (see `asyncapi/doppel-webhooks-asyncapi.yml`) to receive campaign engagement events (email_sent, email_opened, link_visited, data_submitted, email_reported, quiz_passed/failed, call_answered/missed, and voice-behavior labels) as they occur.

## Conventions & errors
- Errors: `{ "message": "..." }`; 401 bad auth; 429 quota. Pagination zero-indexed `page`/`page_size`.
