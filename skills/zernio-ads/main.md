# Skill: Zernio Ads (create & manage ad campaigns)

Gives your Jarvis the ability to create and manage paid ad campaigns across platforms
through the Zernio ads API (campaigns, ad sets, ads, and performance).

Use when the user wants to: launch an ad, build a campaign/ad set/ad, pause or update a
campaign, or read ad performance.

## Read the docs first (source of truth)
- LLM docs: https://docs.zernio.com/llms-full.txt
- OpenAPI:  https://zernio.com/openapi.yaml
- MCP:      https://docs.zernio.com/mcp  (300+ tools auto-generated from the spec — if this
  Jarvis supports MCP, prefer connecting the Zernio MCP over hand-writing fetch tools.)
- Base URL: https://zernio.com/api/v1

## Auth
- Env var: `ZERNIO_API_KEY`
- Header:  `Authorization: Bearer <ZERNIO_API_KEY>`
- Never log the key. Ad spend is real money — confirm with the user before creating/launching.

## Example operations (starting points — get the rest from the docs)
- List ad accounts:   `GET /v1/ads/accounts?accountId=...`  (ad accounts also appear in
  `GET /v1/accounts` as platforms ending in `ads`, e.g. `metaads` — their `_id` is the `accountId`)
- Create ad:          `POST /v1/ads/create`
- Update campaign:    `PUT /v1/ads/campaigns/{id}`
- Update ad set:      `PUT /v1/ads/ad-sets/{id}`
- Performance tree:   `GET /v1/ads/tree?accountId=...`
- Daily performance:  `GET /v1/ads/timeline?accountId=...&fromDate=YYYY-MM-DD&toDate=YYYY-MM-DD` —
  verified live: **the response wraps daily rows as `{ "rows": [ { date, spend, impressions,
  clicks, conversions, ctr, cpc, cpm, roas } ] }`** (unwrap `rows`, not `data`; `400` without
  `accountId`)

## Build it into this Jarvis (Eve conventions)
1. `agent/skills/zernio-ads/SKILL.md` — frontmatter `name: zernio-ads` + when-to-use + auth +
   the "create is real spend, confirm first" rule + "read https://docs.zernio.com/llms-full.txt for the full API."
2. `agent/tools/zernio_ads.ts` — `defineTool()` fetch wrappers for the core ad endpoints,
   `Authorization: Bearer` from `process.env.ZERNIO_API_KEY`. (Or wire the Zernio MCP if supported.)
3. Match this repo's conventions; never hardcode the key; gate launches behind user confirmation.
