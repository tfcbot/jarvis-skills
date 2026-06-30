# Skill: Zernio Scheduler (publish & schedule social posts)

Gives your Jarvis the ability to publish and schedule social posts across 14+ platforms
(Instagram, TikTok, X, LinkedIn, Facebook, YouTube, Threads, and more) via the Zernio API.

Use when the user wants to: publish a post now, schedule a post for later, or check a post's
status across connected accounts.

## Read the docs first (source of truth)
- LLM docs: https://docs.zernio.com/llms-full.txt
- OpenAPI:  https://zernio.com/openapi.yaml
- MCP:      https://docs.zernio.com/mcp  (prefer the Zernio MCP if this Jarvis supports MCP)
- Base URL: https://zernio.com/api/v1

## Auth
- Env var: `ZERNIO_API_KEY`
- Header:  `Authorization: Bearer <ZERNIO_API_KEY>`
- Never log the key.

## Example operations (starting points — get the rest from the docs)
- List connected accounts: `GET /v1/accounts`  (returns accountId / profileId per platform)
- Publish now:             `POST /v1/posts`  body `{ publishNow:true, content, mediaItems:[...], platforms:[...] }`
- Schedule:                `POST /v1/posts`  body `{ scheduledFor:"<ISO-8601>", timezone, content, mediaItems, platforms }`
- Status:                  `GET /v1/posts?id=<post_id>`  (permalinkUrl populates ~1-5 min after publish)

## Build it into this Jarvis (Eve conventions)
1. `agent/skills/zernio-scheduler/SKILL.md` — frontmatter `name: zernio-scheduler` + when-to-use +
   auth + the publishNow-vs-scheduledFor pattern + "read https://docs.zernio.com/llms-full.txt for the full API."
2. `agent/tools/zernio_scheduler.ts` — `defineTool()` wrappers (`list_accounts`, `publish_post`,
   `schedule_post`, `post_status`), `Authorization: Bearer` from `process.env.ZERNIO_API_KEY`.
3. Match this repo's conventions; never hardcode the key.
