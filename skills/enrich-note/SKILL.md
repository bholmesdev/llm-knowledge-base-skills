---
name: enrich-note
description: Enrich a single note with topic tags, source attribution, and links to related notes. Use when asked to enrich, tag, or add metadata to a note.
---
# Enrich note

Enrich one note — the one given by path, or the note in context — with metadata.

If the frontmatter already has `enrichedAt`, the note is done — skip it.

Do the three steps below, then stamp `enrichedAt` with the current ISO timestamp.

## 1. Tags

Read [references/tags.md](references/tags.md) first. Reuse an existing tag whenever one fits. Be **reluctant** to add new tags. Tags should span many notes, not a couple. Only coin a new tag when nothing matches, and when you do, append it to the registry with a one-line description so the next note can reuse it.



Write tags as a frontmatter array of kebab-case topics: `tags: [secret-of-our-success, anthropology]`. Capture the source medium as a tag too when there is one (`book`, `podcast`, `video`, `article`). Leave lifecycle tags like `voice-transcription` alone — they aren't topics.

## 2. Source

If the note has a real origin — a URL, book, podcast, video, or person — record it in frontmatter as a flat `source:` line, plus a `url:` line if you can find one (web research allowed). Most personal notes (lists, reflections, dev logs) have no source; skip it then rather than inventing one.

## 3. Related notes

Find notes elsewhere in the vault that are genuinely related. Use your judgment — grep frontmatter `tags:`, search for key terms, look around.

Add the strong matches to a trailing `## Related` section as wikilinks with readable aliases:

```md
## Related

- [[raw/2026-01-05-the-secret-of-our-success-alloparenting-917xzs8m.md|Alloparenting]]
```

Link **only to notes that exist** — never invent a target.

If nothing is clearly related, **omit the section.**