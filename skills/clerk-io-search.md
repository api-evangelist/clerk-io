---
name: clerk-io-search
description: Run behaviour-ranked product search, search-as-you-type, and query suggestions against a Clerk.io store, with filters, facets and pagination.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - search-search
  - search-predictive
  - search-suggestions
  - search-categories
  - search-pages
  - search-popular
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml + https://docs.clerk.io/docs/filters + https://docs.clerk.io/docs/facets + https://docs.clerk.io/docs/pagenation
---

# Search a Clerk.io store

Clerk.io search ranks on sales and behavioural data as well as keyword matching, so the quality of the
result depends on whether you are feeding it behaviour — see the tracking note below.

## Before you start

- These are all `GET` calls under `https://api.clerk.io/v2`.
- Only the **public** `key` is needed. Do not send `private_key` from a browser.
- `key`, `query` and `limit` are **required** on `search-search` and `search-predictive`.
- **JSON-encode complex query parameters.** `filter`, `exclude`, `facets`, `labels` and `attributes` are
  lists or objects. Sent unencoded in a query string, Clerk.io treats the value as a plain string and you
  get a `ParsingError` or silently wrong results. This is the single most common integration bug.

## Steps

1. **Full search:** `search-search` (`GET /search/search`) with `key`, `query`, `limit`. Set
   `longtail=true` if you want any-word matching instead of all-word.
2. **Search-as-you-type:** `search-predictive` (`GET /search/predictive`) — same parameter shape, tuned
   for an unfinished query. Use `search-suggestions` for query-string autocomplete rather than products.
3. **Hydrate the results.** The response gives `result`, an array of product **IDs**. Pass `attributes`
   with the field names you need and Clerk.io returns them in a parallel `product_data` array in the
   same response. Do not loop and fetch — that is the N+1 this parameter exists to prevent.
4. **Narrow with `filter`** (attribute expression) and `exclude` (array of product IDs).
5. **Facets:** pass `facets` as a list of attribute names to get grouped counts back for the result set.
6. **Paginate with `limit` + `offset`.** `offset` is 0-indexed. For page size N and page P,
   `offset = (P - 1) * N`. There is **no total-count field and no next link** — you know you have hit
   the end only when you get back fewer than `limit` results.
7. **Sort** with `orderby` plus `order` (`asc` / `desc`). Omit both for relevance ranking.
8. **Also searchable:** `search-categories` (categories by relevance and popularity), `search-pages`
   (content pages), and `search-popular` (the most-run queries of the past 2 days — good for a
   zero-state UI).

## Always pass `visitor` and `labels`

Both are optional in the schema and both are marked *"Required for tracking"* in Clerk.io's own
parameter descriptions. Omit them and the call still returns results — but it drops out of Clerk.io
analytics and out of the behavioural signal that ranks future results, so the engine quietly gets worse.
Pass a stable visitor ID, or the literal `auto` to have Clerk.io mint an anonymous one.

## Failure handling

Errors return the vendor envelope, not RFC 9457 problem+json. No `429` is documented and no rate-limit
headers are returned, so back off on any non-`200` and treat `InternalError` as retryable.
