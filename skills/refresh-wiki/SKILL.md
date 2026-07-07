---
name: refresh-wiki
description: Regenerate wikis in the wikis/ directory. Use when asked to update, manage, or refresh the contents of the wiki from new content.
---
# Refresh wiki

Maintain every llm-wiki under `wikis/`, following [Karpathy's knowledge-base pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): raw notes elsewhere in the vault are immutable sources; `wikis/` is the maintained synthesis layer. Each wiki is a directory with its own `AGENTS.md`, `index.md`, `overview.md`, `log.md`, and source/synthesis subdirectories.

For each wiki, do four steps:

## 1. Read the local schema

Read that wiki's `AGENTS.md` and `index.md` first. Its local schema **always wins** over anything here.

## 2. Find new sources

Look for new or changed notes in the vault that fit the wiki's scope — its `AGENTS.md` and existing source pages tell you what belongs. If it's ambiguous whether a note belongs, skip it and record the ambiguity in `log.md` rather than guessing.

## 3. Ingest

For each new source: create or update its summary page, update the relevant concept/person/organization pages, update `index.md` counts and catalog entries, and append to `log.md`. Only touch `overview.md` for durable synthesis changes.

## 4. Lint

A light pass per touched wiki: unresolved wikilinks, orphan pages, stale claims, duplicated concepts, missing source back-links. Fix the easy ones; log the rest as follow-up questions.

Never edit notes outside `wikis/` — raw sources are read-only.
