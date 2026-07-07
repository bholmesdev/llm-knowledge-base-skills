---
name: refresh-wiki-cloud
description: Run refresh-wiki unattended in a cloud sandbox against an Obsidian Sync vault, syncing before and after. Use for scheduled cloud runs.
---
# Refresh wiki (cloud)

The vault is synced to `~/vault` by the environment setup. Run the [refresh-wiki](../refresh-wiki/SKILL.md) process against it, wrapped in a sync.

## 1. Pull the latest vault

```sh
cd ~/vault
ob sync-config --file-types image,audio,video,pdf,unsupported
ob sync
```

## 2. Refresh

Run [refresh-wiki](../refresh-wiki/SKILL.md) against `~/vault/wikis/`. Raw source notes outside `wikis/` are read-only. Never invent facts — attribute claims to source notes, books, people, or organizations.

## 3. Push results back

```sh
cd ~/vault
ob sync
```

Report which wikis were touched, changed pages, skipped candidate notes, and follow-up questions.

If sync reports conflicts or an authentication/setup error at any point, **stop and report the issue** rather than making further edits. Never delete notes, attachments, or wiki pages during scheduled runs.
