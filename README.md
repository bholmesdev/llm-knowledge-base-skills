# LLM knowledge base skills

Agent skills that turn a directory of raw markdown notes into a self-updating knowledge base: tagged and backlinked notes, plus persistent wikis following [Andrej Karpathy's llm-wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

You write things down; agents on a schedule handle the organizing.

## Install

```sh
npx skills add bholmesdev/llm-knowledge-base-skills
```

## The skills

| Skill | What it does |
| --- | --- |
| [enrich-note](skills/enrich-note/SKILL.md) | Enrich one note with topic tags (from a shared [tag registry](skills/enrich-note/references/tags.md)), source attribution, and links to related notes. |
| [enrich-notes-loop](skills/enrich-notes-loop/SKILL.md) | Run enrich-note across every un-enriched note in the vault, unattended. Uses an `enrichedAt` frontmatter stamp to skip finished notes. |
| [refresh-wiki](skills/refresh-wiki/SKILL.md) | Maintain every llm-wiki under `wikis/`: ingest new source notes, update entity/concept pages, and lint for stale claims and orphan pages. |
| [enrich-notes-loop-cloud](skills/enrich-notes-loop-cloud/SKILL.md) | enrich-notes-loop wrapped for scheduled cloud runs: sync an Obsidian vault down, enrich, sync back up. |
| [refresh-wiki-cloud](skills/refresh-wiki-cloud/SKILL.md) | refresh-wiki wrapped the same way for scheduled cloud runs. |

## Expected vault layout

```
your-vault/
├── raw/       # raw, unfiltered notes — agents read these, never rewrite them
└── wikis/     # generated wikis — agents own this layer
```

Tweak the skills to taste if your layout differs.

## Running in the cloud

The `-cloud` variants are built for scheduled runners like [Oz](https://oz.dev). They assume the environment setup has synced your vault to `~/vault` with [Obsidian's headless CLI](https://obsidian.md/help/headless). See the blog post below for the full environment and schedule setup.

## Read the full walkthrough

These skills are from my post on building a self-updating LLM knowledge base: [bholmes.dev/blog](https://bholmes.dev/blog).
