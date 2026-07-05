# Install: Zernio Analytics skill → your Jarvis

Paste this into the coding agent running your Jarvis, from the root of your Jarvis repo.

---

You are installing the **Zernio Analytics** skill into THIS Jarvis. It gives the assistant the
ability to answer social questions from live data — follower counts per platform, recent post
performance, and paid-ads read-outs — via the Zernio API. Read-only; no publishing, no spend.

**Phase 1 — understand this Jarvis (report before writing).** Detect how this project loads
skills and tools. If it is the Eve framework, skills live in `agent/skills/*/SKILL.md`, tools in
`agent/tools/*.ts` via `defineTool()`, and shared clients in `agent/lib/`; otherwise match whatever
pattern exists. Confirm the required env key (`ZERNIO_API_KEY`) is set **in the environment of the
process that runs the agent** — in split UI/brain projects the brain has its own env file, and the
key must be there; a key set only on the UI silently no-ops the tools. State your install plan.

**Phase 2 — build it.** Fetch the playbook at
https://raw.githubusercontent.com/tfcbot/jarvis-skills/v0.2.0/skills/zernio-analytics/main.md
and the live API docs at https://docs.zernio.com/llms-full.txt . Following both, create the skill
file, a small shared Zernio client, and a `social_stats` tool in this repo's conventions, wired to
the env key above. The playbook's **verified endpoints and quirks section is authoritative where it
and the docs index disagree** (several docs-index paths return HTML, and the ads timeline wraps its
rows).

**Phase 3 — finish.** Add the env key placeholder to env/config with a note, name the tool in the
agent's instructions (followers / posts / reach / ad-spend questions route to it), and report what
you created and how to use it.

Constraints: match this repo's conventions, never hardcode secrets, don't break existing skills,
ask before adding any new dependency. This skill is read-only — it must not create posts, campaigns,
or spend.
