# Skill: Scrape Creators (social research)

Gives your Jarvis the ability to research public social media data — profiles, posts,
videos, comments, transcripts, and ad libraries — across TikTok, Instagram, YouTube,
Facebook, X, LinkedIn, Reddit and 20+ platforms, via the Scrape Creators API.

Use when the user wants to: look up a creator/profile, pull a post or video's stats,
search by keyword or hashtag, read a YouTube transcript, or scan an ad library.

## Read the docs first (source of truth)
Implement against the live docs; do not rely on a memorized endpoint list.
- LLM docs: https://docs.scrapecreators.com/llms.txt
- OpenAPI:  https://docs.scrapecreators.com/openapi.json
- Base URL: https://api.scrapecreators.com

## Auth
- Env var: `SCRAPECREATORS_API_KEY`
- Header:  `x-api-key: <SCRAPECREATORS_API_KEY>`
- Read-only and credit-metered; every response includes `credits_remaining`. Never log the key.

## Example operations (starting points — get the rest from the docs)
- TikTok profile:        `GET /v1/tiktok/profile?handle=<handle>`
- TikTok keyword search: `GET /v1/tiktok/search/keyword?query=<q>`
- Instagram profile:     `GET /v1/instagram/profile?handle=<handle>`
- YouTube transcript:    `GET /v1/youtube/video/transcript?url=<url>`
- Facebook ad library:   `GET /v1/facebook/adLibrary/search/ads?query=<q>`

## Build it into this Jarvis (Eve conventions)
1. Create `agent/skills/scrape-creators/SKILL.md` — frontmatter `name: scrape-creators` +
   a `description` covering when to use it, and a short body: auth, the few core operations,
   and "read https://docs.scrapecreators.com/llms.txt for the full endpoint list."
2. Create `agent/tools/scrape_creators.ts` — `import { defineTool } from "eve/tools"`. Expose
   a small set of high-value tools (e.g. `scrape_profile`, `scrape_search`, `scrape_post`,
   `youtube_transcript`) via `defineTool({ description, inputSchema (zod), execute })`, each
   doing `fetch()` against the base URL with the `x-api-key` header from
   `process.env.SCRAPECREATORS_API_KEY`, returning the parsed JSON. Read the docs for exact
   params; keep the tool set small and let Jarvis read the docs for rarer endpoints.
3. Match this repo's existing skill/tool conventions exactly; never hardcode the key;
   surface `credits_remaining` when useful.
