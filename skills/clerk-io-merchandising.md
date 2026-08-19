---
name: clerk-io-merchandising
description: Control Clerk.io search and result ranking with synonyms, redirects, custom search configurations, merchandising rules and curated accessories.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - synonyms-get
  - synonyms-post
  - synonyms-patch
  - synonyms-delete
  - redirects-get
  - redirects-post
  - redirects-patch
  - redirects-delete
  - customized-searches-get
  - customized-searches-post
  - customized-searches-patch
  - customized-searches-delete
  - merchandising-get
  - merchandising-post
  - merchandising-patch
  - merchandising-delete
  - accessories-get
  - accessories-post
  - accessories-patch
  - accessories-delete
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml
---

# Override Clerk.io's ranking on purpose

Everything else in Clerk.io is learned from behaviour. This is the surface where a merchandiser
overrides it — and it is API-driven, so it can live in version control rather than only in the my.clerk.io UI.

## The four levers

1. **Synonyms** (`/synonyms`) — map query terms onto each other so "trainers" finds sneakers. Full CRUD:
   `synonyms-get`, `synonyms-post`, `synonyms-patch`, `synonyms-delete`. Note the write operations mark
   only `key` as required in the schema, but `private_key` is still what authorises the change — send it.
2. **Redirects** (`/redirects/` — the trailing slash is Clerk.io's, keep it) — send a specific query
   straight to a landing page instead of a result set. `redirects-get/post/patch/delete`.
3. **Customised search** (`/customized_search`) — saved search configurations.
   `customized-searches-get/post/patch/delete`.
4. **Merchandising** (`/merchandising`) — the ranking rules applied on top of the behavioural score.
   `merchandising-get/post/patch/delete`.

## Curated accessories

`/accessories` (`accessories-get/post/patch/delete`) holds hand-curated product-to-product associations.
Use it where the behavioural complementary logic (`recommendations-complementary`) has no data yet —
a new product, a long-tail SKU — and let the learned logic take over once volume arrives.

## Operating notes

- Read the current state with the `-get` operation before you write. There is **no idempotency key and
  no optimistic-concurrency header**, so two merchandisers writing at once silently last-write-wins.
- No changelog and no audit endpoint is published; if you need history, keep these definitions in your
  own repo and treat the API as the deploy target.
- All four levers change what shoppers see immediately. There is no staging or preview surface and no
  documented cache TTL.
