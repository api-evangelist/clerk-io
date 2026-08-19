---
name: clerk-io-privacy-requests
description: Service a GDPR subject access or erasure request against Clerk.io using its first-class privacy endpoints.
api: Clerk.io API
base_url: https://api.clerk.io/v2
spec: openapi/clerk-io-openapi.yml
operations:
  - privacy-info
  - privacy-forget
  - customers-delete
generated: '2026-08-13'
method: generated
source: openapi/clerk-io-openapi.yml + https://trust.clerk.io/
---

# Service a GDPR request in Clerk.io

Clerk.io ships data-subject access and erasure as real API endpoints rather than a support ticket. If
you process EU shoppers, wire these into your DSAR workflow instead of emailing support.

## Subject access — what do you hold?

`privacy-info` (`GET /privacy/info`) with required `key` and `private_key`, plus **one** subject
identifier: `email`, `customer` (customer ID) or `visitor` (anonymous visitor ID). Returns what
Clerk.io holds for that subject.

## Erasure — forget them

`privacy-forget` (`GET /privacy/forget`), identical parameter shape. This is destructive and there is no
documented undo, no confirmation step and no idempotency key.

## Doing it correctly

1. Resolve the subject to **every** identifier you hold — a shopper is very often both a `customer` and
   one or more `visitor` IDs. Erasing by `email` alone can leave anonymous behavioural history behind.
2. Call `privacy-info` first for each identifier and keep the response as your evidence of what was
   held.
3. Call `privacy-forget` for each identifier.
4. Call `privacy-info` again to confirm the erasure landed. There is no callback or job status.
5. If the subject is a stored customer record, also `customers-delete` so a later catalog sync does not
   re-create them.

## Notes

- `private_key` is required — this is a server-side flow only, never browser-side.
- Clerk.io publishes GDPR, SOC 2 and ISO 27001 posture at https://trust.clerk.io/.
- Errors use the vendor envelope; an `AuthenticationError` here usually means the private key is scoped
  to a different store than the subject's.
