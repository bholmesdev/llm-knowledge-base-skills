---
name: enrich-notes-loop
description: Enrich every un-enriched note in the vault by running enrich-note over each one. Use to bulk-enrich the vault or auto-tag notes unattended ("while I sleep").
---

# Enrich notes loop

Run the [enrich-note](../enrich-note/SKILL.md) process across the whole vault, one note at a time.

**First run only:** if the tag registry (`enrich-note/references/tags.md`) is still the starter seed, skim the existing notes and flesh it out with the real recurring topics first, so tags are consistent from note one.

Then walk every note under `raw/` (and the vault root) whose frontmatter has no `enrichedAt`, and enrich it. Skip notes already stamped — that's how you know what's left.

This runs unattended: keep going until every note is enriched. Don't delete anything.
