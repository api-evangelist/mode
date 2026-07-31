---
name: Schedule a Mode report and manage subscriptions
description: Create a report schedule and manage delivery subscriptions so stakeholders receive report runs automatically.
api: openapi/mode-openapi-original.yml
operations: [getReport, listReportSchedules, createReportSchedule, getReportSchedule, createSubscriptionForReport, listSubscriptionsForReport]
---

# Schedule a Mode report and manage subscriptions

Automate delivery of a Mode report on a recurring schedule via the REST API (`https://app.mode.com/api`).

## Auth
HTTP Basic with a workspace API token + secret (`authentication/mode-authentication.yml`).

## Steps
1. `getReport` — `GET /{account}/reports/{report}` to confirm the target report.
2. `listReportSchedules` — `GET /{workspace}/reports/{report}/schedules` to see existing schedules.
3. `createReportSchedule` — `POST /{workspace}/reports/{report}/schedules` to add a new recurring run.
4. `getReportSchedule` — `GET /{workspace}/reports/{report}/schedules/{schedule}` to confirm it was created.
5. `createSubscriptionForReport` — `POST /{account}/reports/{report}/subscriptions` to add delivery recipients.
6. `listSubscriptionsForReport` — `GET /{account}/reports/{report}/subscriptions` to verify subscribers.

## Conventions & errors
- Responses are HAL+JSON; POSTs echo the created resource with `_links`.
- No idempotency key — avoid duplicate schedules by checking `listReportSchedules` first.
- Errors use `{ id, message }` (`errors/mode-problem-types.yml`); `403` indicates the token lacks admin privilege for scheduling.
