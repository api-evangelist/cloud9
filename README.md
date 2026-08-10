# Cloud9

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

Cloud9 Esports, Inc. is a North American professional esports organization founded in 2013 by Jack
and Paullie Etienne and headquartered in Santa Monica, California, fielding rosters across League of
Legends, VALORANT, VALORANT Game Changers, Call of Duty and Rainbow Six.

Cloud9 runs no developer program and publishes no OpenAPI. Its public web estate is nonetheless
machine-readable, and that is what this profile captures:

- **[cloud9.gg/wp-json/](https://cloud9.gg/wp-json/)** — an anonymous, unauthenticated WordPress
  REST API. 277 routes across 14 namespaces, including four Cloud9-specific content types that make
  the organization queryable as data: `players` (47), `teams` (45), `achievement` (564) and
  `case-study` (7). Each collection's JSON Schema was harvested verbatim over HTTP `OPTIONS`.
- **[cloud9.gg/wp-json/mcp/mcp-oauth-server](https://cloud9.gg/wp-json/mcp/mcp-oauth-server)** — an
  undocumented remote Model Context Protocol server, discovered by probing `/.well-known/*`. It
  advertises RFC 8414 authorization-server metadata and RFC 9728 protected-resource metadata, and
  is gated behind OAuth 2.1 with PKCE and a single `mcp` scope. Anonymous `tools/list` returns 401,
  so no tool inventory is recorded.
- **[store.cloud9.gg](https://store.cloud9.gg/)** — a Shopify storefront serving the platform's
  default anonymous `/products.json` and `/collections.json`.

The inversion is the interesting part: the agent-native surface is locked while the same content is
served to anyone over REST with no credential at all.

Harvest source: https://forgeglobal.com/cloud9_stock/
