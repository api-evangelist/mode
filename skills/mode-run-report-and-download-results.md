---
name: Run a Mode report and download its results
description: Fetch a report, list its runs, drill into query runs, and download the result content as CSV or JSON.
api: openapi/mode-openapi-original.yml
operations: [getCurrentUser, getReport, listReportRuns, getReportRun, listQueryRunsForReportRun, getReportRunContent]
---

# Run a Mode report and download its results

Use the Mode REST API (`https://app.mode.com/api`) to retrieve a report's latest run and pull down the computed results. Requires a paid Mode Business workspace.

## Auth
HTTP Basic. Use the API token as the username and the secret as the password, base64-encoded into `Authorization: Basic <encoded>`. Confirm the caller with `getCurrentUser` (`GET /account`). See `authentication/mode-authentication.yml`.

## Steps
1. `getReport` — `GET /{account}/reports/{report}` to confirm the report exists and read its metadata (HAL `_links` point to runs and queries).
2. `listReportRuns` — `GET /{account}/reports/{report}/runs` (paginate with `page`/`per_page`) to find the run of interest; or take the most recent completed run.
3. `getReportRun` — `GET /{account}/reports/{report}/runs/{run}` to check the run state before downloading.
4. `listQueryRunsForReportRun` — `GET /{account}/reports/{report}/runs/{run}/query_runs` if you need per-query results.
5. `getReportRunContent` — `GET /{account}/reports/{report}/runs/{run}/results/content.csv` (or `.json`) to download the result set.

## Conventions & errors
- Responses are `application/hal+json`; follow `_links`/`_embedded` for navigation (`conventions/mode-conventions.yml`).
- On failure expect `{ id, message }` with `id` one of `bad_request`/`unauthorized`/`forbidden`/`not_found` (`errors/mode-problem-types.yml`). A `401` means bad/missing credentials; `403` means the token lacks privilege for the resource.
- No idempotency contract — these are read-only GETs, safe to retry.
