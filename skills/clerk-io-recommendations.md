---
name: clerk-io-recommendations
description: Choose and call the right Clerk.io recommendation logic for a storefront slot — popular, trending, complementary, substituting, or visitor- and customer-personalised.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - recommendations-popular
  - recommendations-trending
  - recommendationsnew
  - recommendationscurrently_watched
  - recommendationsrecently_bought
  - recommendations-complementary
  - recommendations-substituting
  - recommendationsmost_sold_with
  - recommendations-bundle
  - recommendations-keywords
  - recommendations-category-popular
  - recommendationscategorytrending
  - recommendationscategorynew
  - recommendationscategorypopular_subcategories
  - recommendations-visitor-history
  - recommendations-visitor-complementary
  - recommendations-visitor-substituting
  - recommendations-customer-history
  - recommendations-customer-complementary
  - recommendations-customer-substituting
  - recommendationspageproduct
  - recommendationspagecategory
  - recommendationspagesubstituting
  - recommendationspagerelated_products
  - recommendationspagerelated_categories
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml + https://docs.clerk.io/llms.txt
---

# Pick the right Clerk.io recommendation logic

There are 25 recommendation operations and they are not interchangeable. The logic you pick *is* the
product decision; the parameters barely change between them.

## Shared contract

- `GET` under `https://api.clerk.io/v2`. Public `key` and `limit` are required on every logic.
- `visitor` and `labels` are marked *"Required for tracking"* — pass them or the slot never shows up in
  analytics and never trains the model. `labels` should name the slot, e.g. `["Home Page / Popular"]`.
- `attributes` hydrates product fields into `product_data` so you do not have to fetch each ID.
- `filter` and `exclude` work here exactly as they do in search, and must be JSON-encoded in the query
  string.

## Choose by slot

| Storefront slot | Operation | Needs |
|---|---|---|
| Home page, no context | `recommendations-popular` | nothing |
| Home page, "what's hot now" | `recommendations-trending` | nothing |
| New arrivals | `recommendationsnew` | nothing |
| Live social proof | `recommendationscurrently_watched` | nothing |
| Live social proof, purchases | `recommendationsrecently_bought` | nothing |
| Product page, cross-sell | `recommendations-complementary` | `products` |
| Product page, alternatives | `recommendations-substituting` | `products` |
| Cart, "bought together" | `recommendationsmost_sold_with` | `products` |
| Cart, bundle offer | `recommendations-bundle` | `products` |
| Out-of-stock swap | `recommendations-substituting` | `products` |
| Category page | `recommendations-category-popular` / `...trending` / `...new` | `categories` |
| Category nav | `recommendationscategorypopular_subcategories` | `categories` |
| Blog / content page | `recommendations-keywords` | keyword content |
| Anonymous return visit | `recommendations-visitor-complementary` | `visitor` |
| Anonymous "you viewed" | `recommendations-visitor-history` | `visitor` |
| Logged-in / email | `recommendations-customer-complementary` | `customer` |
| Logged-in "you bought" | `recommendations-customer-history` | `customer` |
| Content page relations | `recommendationspageproduct`, `recommendationspagerelated_products`, `recommendationspagecategory`, `recommendationspagerelated_categories`, `recommendationspagesubstituting` | `page` |

## Visitor vs customer

They are different logics reading different signals. `recommendations-visitor-*` reads **on-site
activity** for an anonymous visitor ID. `recommendations-customer-*` reads **order history** for a known
customer and is what you want in an email or a logged-in session. Using the visitor logic for a known
customer throws away the order history you paid to ingest.

## Notes

- Response is `result` (array of product IDs) plus `product_data` when you asked for `attributes`.
- Some logics accept `debug` and `callback`; `callback` is JSONP and should not be used in new work.
- No idempotency concerns — these are all reads — but there is no documented rate limit either, so a
  page rendering ten slots is making ten uncoordinated calls with no throttling signal.
