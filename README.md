# Jarvis Skills

Installable capability skills for your Jarvis (an Eve-framework AI assistant).

Each skill is a thin **installer prompt** you paste into the coding agent running your Jarvis. The installer reads the skill's playbook (`main.md`) plus the official API docs, then builds the capability into your assistant (an `agent/skills/<name>/SKILL.md` + `agent/tools/<name>.ts`), wired to your own API key. Nothing is hardcoded or pre-compiled — your agent resolves the integration from the live docs, so skills never go stale.

## Install a skill
1. Open the skill's `install.md` (e.g. `skills/scrape-creators/install.md`).
2. Paste it into the coding agent at the root of your Jarvis repo.
3. Add the API key it asks for.

## Skills
| Skill | What it gives Jarvis | Docs |
|---|---|---|
| `scrape-creators` | research public social data across 20+ platforms | docs.scrapecreators.com |
| `shopify-admin` | manage a Shopify store (products, prices, orders) | shopify.dev |
| `zernio-ads` | create & manage paid ad campaigns | docs.zernio.com |
| `zernio-scheduler` | publish & schedule social posts (14+ platforms) | docs.zernio.com |

_Version: 0.1.0_
