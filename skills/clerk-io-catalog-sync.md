---
name: clerk-io-catalog-sync
description: Sync a store's products, categories and content pages into Clerk.io so search and recommendations have a current catalog to rank.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - products-post
  - products-patch
  - categories-get-1
  - categories-get-1-1
  - categories-post
  - categories-patch
  - pages-post
  - pages-patch
  - pages-get
  - pages-delete
  - product-add
  - product-remove
  - product-attributes
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml + https://docs.clerk.io/reference/product-resource
---

# Sync a catalog into Clerk.io

Clerk.io ranks nothing it has not been given. Every search result and every recommendation is drawn from
the products, categories and pages you push into this API.

## Before you start

- Base URL is `https://api.clerk.io/v2`. There is no other version.
- You need **both** keys. The public `key` identifies the store; `private_key` authenticates it and is
  required for every operation on this page. **Only ever send `private_key` over HTTPS.**
- IDs are **yours**. Clerk.io mints nothing — `product.id` and `category.id` are the IDs from your own
  e-commerce platform, and every write is an upsert against them.

## Steps

1. **Push categories first.** `categories-post` (`POST /categories`) with a body of
   `{key, private_key, categories}`. Products reference category IDs, so categories must exist first or
   the product's `categories` list points at nothing.
2. **Push products.** `products-post` (`POST /products`) with `{key, private_key, products}`. Every
   product object must carry `id`, `name`, `description`, `price`, `image`, `url`, `categories` and
   `created_at` (unix timestamp). `list_price` is optional but is what makes discount rendering work.
   Beyond the required core, a product is an open JSON object — add any attribute you want to filter,
   facet or template on. Nested objects are allowed but **cannot be used for filtering**.
3. **Batch, don't drip.** `products` is an array. Send the catalog in batches rather than one product
   per call.
4. **Patch, don't repush, for small changes.** `products-patch` (`PATCH /products`) takes partial
   product objects — use it for price and stock churn.
5. **Push content pages** with `pages-post` if you want page search (`search-pages`) or page-based
   recommendation logics.
6. **Single-product corrections** use `product-add` and `product-remove` (both `GET`) rather than a full
   batch write.
7. **Verify** with `product-attributes` (`GET /product/attributes`) to confirm which attributes Clerk.io
   actually holds for a product before you template against them.

## Things that will bite you

- **There is no idempotency key.** Clerk.io publishes no idempotency header and no replay detection.
  Re-sending an identical batch converges because writes are upserts keyed on your `id`, but a batch
  that fails halfway leaves you with no way to tell what landed. Re-send the whole batch.
- **There are no published rate limits** — and no `RateLimit-*` or `Retry-After` headers to react to.
  Self-throttle your backfill; you are flying blind. See `rate-limits/clerk-io-rate-limits.yml`.
- **`GET /products` and `DELETE /products` carry the operationIds `categories-get-1` and
  `categories-get-1-1`** in Clerk.io's published spec. Those are provider copy-paste artifacts, not a
  mistake in this skill. Match on method + path, not on the ID.
- **Errors are not RFC 9457.** A failure returns `{"status":"error","message","moreInfo","type","id"}`.
  `ParsingError` means malformed JSON or a missing/mistyped field; `AuthenticationError` means a bad key
  or a disabled account. Keep the `moreInfo` link — it expires 24 hours after issue if never opened.
  See `errors/clerk-io-problem-types.yml`.
