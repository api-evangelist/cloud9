---
name: Read the Cloud9 roster, teams, record and news
description: >-
  Pull the Cloud9 Esports roster, team pages, competitive achievements, partner case studies and
  news from the anonymous WordPress REST API on cloud9.gg. No credential of any kind is required.
  Use this when an agent needs current, structured Cloud9 organizational data instead of scraping
  the marketing site.
api: json-schema/cloud9-wp-rest-index.json
base_url: https://cloud9.gg/wp-json/wp/v2
auth: none
routes:
  - GET /wp/v2/players
  - GET /wp/v2/players/{id}
  - GET /wp/v2/teams
  - GET /wp/v2/teams/{id}
  - GET /wp/v2/achievement
  - GET /wp/v2/case-study
  - GET /wp/v2/posts
  - GET /wp/v2/categories
  - GET /wp/v2/media/{id}
  - GET /wp/v2/search
generated: '2026-08-09'
method: generated
source: probed https://cloud9.gg/wp-json/ (277 routes) and HTTP OPTIONS schemas
---

# Read the Cloud9 roster, teams, record and news

Cloud9 publishes no developer program, no API key flow and no documentation. It does serve a
fully anonymous WordPress REST API. Every route below was probed and returned `200` without a
credential on 2026-08-09. Do not invent routes — the authoritative list is the discovery index at
`https://cloud9.gg/wp-json/`, captured verbatim in `json-schema/cloud9-wp-rest-index.json`, and
each collection's field list is in `json-schema/cloud9-wp-rest-schemas.json`.

## Before you start

- Base URL: `https://cloud9.gg/wp-json/wp/v2`
- Authentication: **none**. Send no `Authorization` header. If you receive `401 rest_forbidden`
  you have hit a management route (`/settings`, `/plugins`, `/wp-abilities/v1/*`,
  `/mcp/*`) — those are not part of the public surface. Back off, do not retry with credentials.
- Rate limits: none are published and no `RateLimit-*` headers are returned. Be a good citizen:
  page with `per_page=100`, cache results, and do not hammer `/media`.
- There is **no idempotency contract**. This skill is read-only; do not attempt writes.

## Step 1 — Get the roster

```
GET /wp/v2/players?per_page=100&_fields=id,slug,title,link,acf
```

`X-WP-Total` on the response tells you the roster size (47 at capture). Each item is a WordPress
post of type `players`; the player-specific fields (game, role, handle, socials) live under the
`acf` object supplied by Advanced Custom Fields. Inspect `acf` on a single item first — the field
group is site-defined and is **not** in the JSON Schema, so treat its keys as discovered, not
guaranteed.

## Step 2 — Get the teams

```
GET /wp/v2/teams?per_page=100&_fields=id,slug,title,link
```

45 items at capture. Slugs map onto the public team pages, e.g. `league-of-legends`,
`valorant`, `valorant-game-changers`, `call-of-duty`, `rainbow-6`, `counterstrike`,
`rocket-league`, `halo`, `super-smash-bros-melee`, `heroes-of-the-storm`, `london-spitfire`,
`streamers-content-creators`.

## Step 3 — Get the competitive record

```
GET /wp/v2/achievement?per_page=100&page=N&_fields=id,title,date,link
```

564 items at capture, so expect 6 pages at `per_page=100`. Read `X-WP-TotalPages` from the first
response and iterate `page` up to that value rather than guessing. Stop when the header says so —
requesting a page beyond the last returns `400 rest_invalid_param`.

## Step 4 — Get partner case studies and news

```
GET /wp/v2/case-study?per_page=100&_fields=id,title,excerpt,link
GET /wp/v2/posts?per_page=20&orderby=date&order=desc&_embed
```

`_embed` inlines the featured image and terms under `_embedded`, saving a round trip to
`/wp/v2/media/{id}`. Use `categories` (from `GET /wp/v2/categories`, 19 at capture) to filter news
by topic.

## Step 5 — Search across everything

```
GET /wp/v2/search?search=<term>&per_page=20&subtype=players,teams,achievement,post
```

769 objects are indexed. The search result items are thin — `id`, `title`, `url`, `type`,
`subtype` — so follow `_links.self` to fetch the full record.

## Error handling

The envelope is WordPress's, not RFC 9457:

```json
{"code": "rest_post_invalid_id", "message": "Invalid post ID.", "data": {"status": 404}}
```

Branch on `code`, not on the message string. The codes you will actually see are catalogued in
`errors/cloud9-problem-types.yml`: `rest_no_route` (404), `rest_post_invalid_id` (404),
`rest_invalid_param` (400, with per-parameter reasons under `data.details`), and
`rest_forbidden` / `rest_cannot_view_plugins` (401 on management routes).

## What this skill deliberately does not do

- It does not touch `https://cloud9.gg/wp-json/mcp/mcp-oauth-server`. That MCP server exists but
  is gated behind OAuth 2.1 with a single `mcp` scope, and its tool list cannot be enumerated
  anonymously. See `mcp/cloud9-mcp.yml`.
- It does not write. Write methods are advertised on these same routes but require a WordPress
  application password belonging to a Cloud9 site user.
- It does not treat the `jetpack/*`, `my-jetpack/*`, `wp-rocket/*` or `akismet/*` namespaces as
  content. They are hosting and plugin management surfaces.
