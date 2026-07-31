---
name: Explore Mode data sources and run datasets
description: List connected data sources, inspect a dataset, trigger a dataset run, and read the run result.
api: openapi/mode-openapi-original.yml
operations: [listDataSources, getDataSource, getDataset, runDataset, listDatasetRuns, getDatasetRun]
---

# Explore Mode data sources and run datasets

Inventory a workspace's connected warehouses and refresh a dataset via the Mode REST API (`https://app.mode.com/api`).

## Auth
HTTP Basic with API token + secret (see `authentication/mode-authentication.yml`). Workspace API tokens grant admin-level access.

## Steps
1. `listDataSources` — `GET /{workspace}/data_sources` to enumerate connected databases/warehouses.
2. `getDataSource` — `GET /{workspace}/data_sources/{data_source}` to read connection metadata.
3. `getDataset` — `GET /{account}/datasets/{dataset}` to inspect a dataset definition.
4. `runDataset` — `POST /{account}/datasets/{dataset}/runs` to trigger a fresh run (returns `202 Accepted` while it executes).
5. `listDatasetRuns` / `getDatasetRun` — poll `GET /{account}/datasets/{dataset}/runs` then `GET /{account}/datasets/{dataset}/runs/{run}` until the run completes.

## Conventions & errors
- Long-running operations return `202`; poll the run resource for completion.
- Paginate list calls with `page`/`per_page`.
- Errors use the `{ id, message }` envelope (`errors/mode-problem-types.yml`).
