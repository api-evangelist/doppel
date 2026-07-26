---
name: Scan a URL for suspiciousness with Doppel
description: Submit a URL for analysis and retrieve its low/medium/high suspiciousness result.
api: openapi/doppel-openapi-original.yml
operations: [scan, get_scan_result]
---

# Scan a URL for suspiciousness with Doppel

## Auth
`x-api-key` + `x-user-api-key` headers (Doppel Vision API settings).

## Steps
1. **Submit the scan** — `scan` (`POST /scan`) with the target URL. Doppel analyzes it and categorizes suspiciousness as low, medium, or high; a high result can spawn an alert for triage. Returns a scan id (HTTP 201).
2. **Get the result** — `get_scan_result` (`GET /scan/result`) with the submitted scan's id. Returns the suspiciousness categorization. 404 if the id is unknown.

## Conventions & errors
- 400 for an invalid/protected URL; 401 for bad credentials; 429 on quota; envelope is `{ "message": "..." }`.
- For bulk passive intake instead of a single scan, use `submit-referrer-logs` (`POST /alert/referrer`) — it submits referrer URLs for background analysis (HTTP 202) with no guarantee an alert is created.
