# Skill: Zernio Analytics (followers, post performance, ads read-outs)

Gives your Jarvis the ability to answer social questions from live Zernio data: follower counts
per platform ("how many Instagram followers do I have"), recent post performance (impressions,
likes, comments, clicks), and paid-ads totals (spend, impressions, clicks, conversions).

Use when the user wants to: read follower counts, check how posts are doing, see reach/engagement,
or read ad spend and performance. **Read-only** — publishing belongs to `zernio-scheduler` and
campaign management to `zernio-ads`.

## Read the docs first (source of truth)
- LLM docs: https://docs.zernio.com/llms-full.txt
- OpenAPI:  https://zernio.com/openapi.yaml
- MCP:      https://docs.zernio.com/mcp  (prefer the Zernio MCP if this Jarvis supports MCP)
- Base URL: https://zernio.com/api/v1

## Auth
- Env var: `ZERNIO_API_KEY`
- Header:  `Authorization: Bearer <ZERNIO_API_KEY>`
- Never log the key. Read-only endpoints — no spend, no writes, no confirmation gate needed.

## Verified endpoints (tested against the live API)
- Connected accounts:   `GET /v1/accounts` — one row per account with `platform`, `username`,
  `followersCount`, `adsStatus`. **Ad accounts appear here too**, as platforms ending in `ads`
  (e.g. `metaads`) — their `_id` is the `accountId` the ads timeline needs.
- Follower stats:       `GET /v1/accounts/follower-stats` — follower counts across accounts;
  merge into the accounts list by account id (fall back to platform+username).
- Post analytics:       `GET /v1/analytics?platform=<p>&fromDate=YYYY-MM-DD&toDate=YYYY-MM-DD` —
  per-platform post rows: `{ content, publishedAt, analytics: { impressions, reach, views, likes,
  comments, shares, saves, clicks, engagementRate } }`.
- Ads timeline:         `GET /v1/ads/timeline?accountId=<adAccountId>&fromDate=&toDate=` — daily
  ad metrics. **The response wraps the rows as `{ "rows": [ { date, spend, impressions, reach,
  clicks, conversions, ctr, cpc, cpm, actions, roas } ] }`** — unwrap `rows`, not `data`. Returns
  `400` without `accountId`.

## Quirks that will bite you (all verified)
- The docs-index paths `/analytics/posts` and `/adcampaigns/get-ads-timeline` return the marketing
  site's **HTML with status 200**, not JSON. Use the endpoints above.
- **TikTok reports `views` while `impressions` stays 0; Instagram reports `impressions`.** For a
  cross-platform "impressions" number, take `max(impressions, views, reach)` per post.
- Post captions arrive with raw newlines — collapse whitespace for list rows.
- Let each slice degrade independently: a failing ads read should not blank followers, and vice
  versa.

## Shape the output for the model
Return **per-platform follower totals** (e.g. `followersByPlatform: { instagram: 8, tiktok: 270 }`)
plus a grand total, so "how many Instagram followers" is a direct lookup — not a sum the model has
to compute from raw account rows.

## Build it into this Jarvis (Eve conventions)
1. `agent/skills/zernio-analytics/SKILL.md` — frontmatter `name: zernio-analytics` + when-to-use +
   auth + "this is the source of truth for social numbers; never guess them" + "read
   https://docs.zernio.com/llms-full.txt for the full API."
2. `agent/lib/zernio.ts` — a small shared client (bearer fetch, tolerant field picking, the
   endpoints above) so future Zernio tools reuse it.
3. `agent/tools/social_stats.ts` — one `defineTool()` returning `{ followersTotal,
   followersByPlatform, accounts, recentPosts, ads }`, guarded: if `ZERNIO_API_KEY` is unset,
   return `{ configured: false, message: "…" }` instead of failing.
4. Name the tool in `agent/instructions.md` (followers / posts / reach / ad-spend questions route
   to it) so the agent knows to reach for it.
5. Match this repo's conventions; never hardcode the key.
