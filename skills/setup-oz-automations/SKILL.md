---
name: setup-oz-automations
description: Set up Oz scheduled agents that run enrich-notes-loop-cloud and refresh-wiki-cloud against an Obsidian Sync vault. Use when asked to set up knowledge base automations in the cloud.
---
# Set up Oz automations

Create the Oz environment and schedules that run [enrich-notes-loop-cloud](../enrich-notes-loop-cloud/SKILL.md) nightly and [refresh-wiki-cloud](../refresh-wiki-cloud/SKILL.md) weekly against the user's Obsidian Sync vault.

## 0. Prerequisites

Check whether the Oz platform skill is installed — it teaches the `oz` CLI. If it's absent, install it:

```sh
npx skills add warpdotdev/oz-agent-skill
```

Read that skill before running any `oz` commands.

Then gather from the user:

- **Obsidian Sync vault name** — e.g. `Main`. They need an [Obsidian Sync](https://obsidian.md/sync) vault pointed at their notes directory, and the [headless CLI](https://obsidian.md/help/headless) auth token for it.
- **Schedule times** — default to nightly enrichment and weekly wiki refresh if they have no preference.

The skills are pulled straight from `bholmesdev/llm-knowledge-base-skills` — no repo of their own needed. Only ask about a repo if the user says they've forked the skills to tweak them; then substitute their `owner/name` everywhere `bholmesdev/llm-knowledge-base-skills` appears below.

Make sure they're logged in (`oz login`). If they have no account, send them to [oz.dev](https://oz.dev) → "start an agent".

## 1. Store the Obsidian auth token as a secret

```sh
oz secret create --personal OBSIDIAN_AUTH_TOKEN
```

The user pastes their token when prompted — never ask them to put it in the conversation.

## 2. Create the environment

The setup commands install the Obsidian headless CLI and sync the vault to `~/vault` on boot:

```sh
oz environment create \
  --personal \
  --name "Knowledge base automation" \
  --docker-image "warpdotdev/dev-base:latest-agents" \
  --repo "bholmesdev/llm-knowledge-base-skills" \
  --setup-command 'npm install -g obsidian-headless' \
  --setup-command 'mkdir -p ~/.obsidian-headless && printf "%s" "$OBSIDIAN_AUTH_TOKEN" > ~/.obsidian-headless/auth_token' \
  --setup-command 'mkdir -p ~/vault && cd ~/vault && ob sync-setup --vault VAULT_NAME && ob sync'
```

Get the environment ID with `oz environment list` for the next step.

## 3. Create the schedules

One nightly run for note enrichment, one weekly for wikis. Each points at its cloud skill, running on Kimi k2.6 — an open-weight model that's token-efficient and plenty capable for this work (swap `--model` if the user prefers something else; `oz model list` shows the options):

```sh
oz schedule create \
  --personal \
  --name "Enrich notes loop" \
  --cron "0 8 * * *" \
  --environment "ENV_ID" \
  --model kimi-k26-fireworks \
  --skill "bholmesdev/llm-knowledge-base-skills:enrich-notes-loop-cloud" \
  --prompt 'Run the enrich-notes-loop-cloud skill.'

oz schedule create \
  --personal \
  --name "Refresh wikis" \
  --cron "0 8 * * 1" \
  --environment "ENV_ID" \
  --model kimi-k26-fireworks \
  --skill "bholmesdev/llm-knowledge-base-skills:refresh-wiki-cloud" \
  --prompt 'Run the refresh-wiki-cloud skill.'
```

## 4. Wrap up

Tell the user:

- Both schedules are live, with names, cron times, and schedule IDs.
- They can pause one anytime with `oz schedule pause SCHEDULE_ID`.
- Every run is viewable in the online portal at `https://oz.warp.dev/runs/RUN_ID`. Once runs exist, get their IDs with `oz run list --output-format json` (or `oz schedule get SCHEDULE_ID`, which shows a schedule's recent runs) and output the full portal URLs so the user can click straight through.

Offer to pause the schedules initially if they'd rather test with a manual run first. If they do want a test run, spawn it with `oz agent run-cloud`, then report its `https://oz.warp.dev/runs/RUN_ID` link so they can watch it live.
