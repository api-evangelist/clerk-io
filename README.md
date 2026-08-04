# Clerk.io (clerk-io)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Clerk.io is an e-commerce personalization platform that uses artificial intelligence and machine learning to deliver tailored product recommendations, on-site search results, audience-segmented email campaigns, and merchandising controls for online retailers. The platform exposes a REST API for product, category, order, and customer data ingestion, plus client-side JavaScript and Liquid templating for recommendation slots and search experiences.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/clerk-io/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/clerk-io/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- AI
- Commerce
- E-Commerce
- Email Marketing
- Personalization
- Recommendations
- Search

## Timestamps

- **Created:** 2025-02-08
- **Modified:** 2026-04-26

## APIs

### Clerk.io API

The Clerk.io API provides REST endpoints for managing products, categories, orders, customers, recommendations, and search. The API uses a dual-key authentication model: a public key identifies the store and is used in browser-side requests, while a private key is required for sensitive operations and data ingestion. JSON is the primary payload format and SSL is required when sending the private key.

- **Human URL:** [https://docs.clerk.io/](https://docs.clerk.io/)

#### Tags

- Commerce
- Personalization
- Recommendations
- Search

#### Properties

- [Documentation](https://docs.clerk.io/)
- [Getting Started](https://docs.clerk.io/docs/how-the-clerkio-platform-works)
- [Authentication](https://docs.clerk.io/docs/authentication)
- [Errors](https://docs.clerk.io/docs/errors)
- [Pagination](https://docs.clerk.io/docs/pagenation)

### Clerk.js Client Library

Clerk.js is the browser-side JavaScript library for embedding Clerk.io recommendation slots, search, and email opens on a storefront, with Liquid templating support and event tracking.

- **Human URL:** [https://docs.clerk.io/docs/clerkjs-quick-start](https://docs.clerk.io/docs/clerkjs-quick-start)

#### Tags

- Client Library
- JavaScript

#### Properties

- [Documentation](https://docs.clerk.io/docs/clerkjs-quick-start)

## Common Properties

- [GitHub Organization](https://github.com/clerkio)
- [LinkedIn](https://www.linkedin.com/company/clerk-io)
- [Website](https://www.clerk.io/)
- [Documentation](https://docs.clerk.io/)
- [Knowledgebase](https://help.clerk.io/)
- [Status Page](https://status.clerk.io/)
- [Blog](https://www.clerk.io/blogs)
- [Pricing](https://www.clerk.io/pricing)
- [Partners](https://www.clerk.io/partners)
- [Integrations](https://www.clerk.io/integrations)
- [Trust  Center](https://trust.clerk.io/)
- [Terms of Service](https://www.clerk.io/terms-of-service)
- [Privacy Policy](https://www.clerk.io/privacy)
- [JSON-LD](json-ld/clerk-io-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Spectral Rules](rules/clerk-io-rules.yml) — [Spectral](https://docs.stoplight.io/docs/spectral)
- [L L Ms Txt](https://docs.clerk.io/llms.txt)

## Maintainers

**FN:** Kin Lane
**Email:** kinlane@gmail.com
