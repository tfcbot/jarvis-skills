# Install: Scrape Creators skill → your Jarvis

Paste this into the coding agent running your Jarvis, from the root of your Jarvis repo.

---

You are installing the **Scrape Creators** skill into THIS Jarvis. It gives the assistant
the ability to research public social data (profiles, posts, videos, comments, ad libraries)
across 20+ platforms.

**Phase 1 — understand this Jarvis (report before writing).** Detect how this project loads
skills and tools. If it is the Eve framework, skills live in `agent/skills/*/SKILL.md` and
tools in `agent/tools/*.ts` via `defineTool()`; otherwise match whatever pattern exists.
Confirm `SCRAPECREATORS_API_KEY` is set in env, or tell the user to add it (get a key at
scrapecreators.com). State your install plan.

**Phase 2 — build it.** Fetch the playbook at
https://raw.githubusercontent.com/tfcbot/jarvis-skills/v0.2.0/skills/scrape-creators/main.md
and the live API docs at https://docs.scrapecreators.com/llms.txt. Following both, create the
skill file and a small set of tool wrappers in this repo's conventions, wired to
`SCRAPECREATORS_API_KEY` (`x-api-key` header). Keep the tool set focused; the agent can read
the docs for rarer endpoints at runtime.

**Phase 3 — finish.** Add the `SCRAPECREATORS_API_KEY` placeholder to env/config with a note,
and report what you created and how to use it.

Constraints: match this repo's conventions, never hardcode the key, don't break existing
skills, ask before adding any new dependency.
