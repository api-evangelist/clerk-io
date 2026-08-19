---
name: clerk-io-order-and-behaviour-ingest
description: Feed Clerk.io the sales, customer and on-site behaviour data its ranking depends on — orders, parcels, customers, and the /log event surface.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - orders-post
  - orders-patch
  - orders-get
  - orders-delete
  - parcels-post
  - parcels-get
  - parcels-patch
  - parcels-delete
  - customers-post
  - customers-patch
  - customers-get
  - customers-delete
  - log-click
  - logproduct
  - logcategory
  - logcartadd
  - logcartremove
  - logcartupdate
  - log-sale
  - logsale-copy
  - logreturned
  - log-email
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml + https://docs.clerk.io/reference/order-resource + https://docs.clerk.io/docs/visitor-tracking
---

# Feed Clerk.io the behaviour that ranks the catalog

Clerk.io is a behavioural engine. A synced catalog with no orders and no events ranks on keywords alone.
This is the half of the integration that makes the other half good.

## Orders

- `orders-post` (`POST /orders`) with `{key, private_key, orders}`. Each order needs `id`, `products`
  and `time`; add `customer` or `email` to attach it to a known customer — that is what enables the
  `recommendations-customer-*` logics.
- `orders-patch` for corrections, `orders-delete` (`GET`-style params: `key`, `private_key`, `orders`)
  for removals. Note `orders-get` and `orders-delete` take their arguments as **query parameters**, and
  `orders` must be JSON-encoded there.
- **Backfill your order history** before go-live. Personalisation quality on day one is a function of
  how much history you loaded.

## Parcels

`parcels-post` / `parcels-get` / `parcels-patch` / `parcels-delete` on `/orders/parcels` bind one or more
shipment parcels to an existing Order. Create the order first.

## Customers

`customers-post` with `{key, private_key, customers}`. A customer object needs **either** `customerid`
**or** `email`; `email` is required if you use the Email or Audience products. `subscribed` carries
consent state. Like products, a customer object is open — any extra attribute you add becomes available
for audience segmentation.

## Behaviour events

All under `/log/`, all requiring the public `key` plus a `visitor` (or `customer`/`email`) to attribute to:

- `log-click` — a click on a Clerk.io-rendered product. Clerk.js does this for you on elements it
  rendered; you only need it for markup you rendered yourself.
- `logproduct` — a product page view. `logcategory` — a category view.
- `logcartadd` / `logcartremove` / `logcartupdate` — cart state changes.
- `log-sale` (`GET`) or `logsale-copy` (`POST /log/sale`) — a completed sale, with `sale` and `products`
  required. Use the POST form for anything non-trivial; the GET form will hit URL-length limits.
- `logreturned` — a return, so Clerk.io stops promoting a product your customers send back.
- `log-email` — an email open, which is how the Email product is metered.

## Warnings

- Every one of these is an unauthenticated-from-the-browser public-key call except the order and
  customer writes, which need `private_key`. **Never put `private_key` in client-side code.**
- No idempotency key exists. Logging the same sale twice double-counts it in the behavioural model;
  guard on your side.
- `/log/sale` exists as both `GET` and `POST` with different operationIds (`log-sale`, `logsale-copy`) —
  that is what the provider ships.
