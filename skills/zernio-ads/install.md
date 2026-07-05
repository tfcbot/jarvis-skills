# Install: Zernio Ads skill → your Jarvis

Paste this into the coding agent running your Jarvis, from the root of your Jarvis repo.

---

You are installing the **Zernio Ads** skill into THIS Jarvis. It gives the assistant the ability to create and manage paid ad campaigns across platforms via the Zernio ads API.

**Phase 1 — understand this Jarvis (report before writing).** Detect how this project loads
skills and tools. If it is the Eve framework, skills live in `agent/skills/*/SKILL.md` and tools
in `agent/tools/*.ts` via `defineTool()`; otherwise match whatever pattern exists. Confirm the
required env key(s) are set (ZERNIO_API_KEY) **in the environment of the process that runs the
agent** — in split UI/brain projects the brain has its own env file, and the key must be there (a
key set only on the UI silently no-ops the tools) — or tell the user to add them. State your
install plan.

**Phase 2 — build it.** Fetch the playbook at
https://raw.githubusercontent.com/tfcbot/jarvis-skills/v0.2.0/skills/zernio-ads/main.md
and the live API docs at https://docs.zernio.com/llms-full.txt . Following both, create the skill file and a small set of tool
wrappers in this repo's conventions, wired to the env key(s) above. Keep the tool set focused;
the agent can read the docs for rarer endpoints at runtime.

**Phase 3 — finish.** Add the env key placeholder(s) to env/config with a note, and report what
you created and how to use it.

Constraints: match this repo's conventions, never hardcode secrets, don't break existing skills,
ask before adding any new dependency. For actions that spend money or post publicly, confirm with
the user first.
