# Wiki Schema

## Domain
Dimitris's personal AI operating system: Hermes Agent, local-first AI, VPS dashboard, Obsidian notes, automation workflows, local models, and research/strategy that should compound over time.

## Conventions
- File names: lowercase, hyphens, no spaces.
- Every wiki page starts with YAML frontmatter.
- Use `[[wikilinks]]` to connect related pages.
- Every new or updated page must be listed in `index.md`.
- Every action must be appended to `log.md`.
- Keep raw source files immutable after ingestion.
- Prefer concise, practical notes in Greek/Greeklish-friendly language where useful.

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [tag1, tag2]
sources: [raw/videos/source-file.md]
confidence: high | medium | low
---
```

## Tag Taxonomy
- hermes
- memory
- obsidian
- llm-wiki
- local-ai
- vps
- dashboard
- coding-agent
- automation
- research
- youtube
- workflow
- knowledge-base
- tailscale
- ollama

## Page Thresholds
- Create a page when a concept/tool is central to a source or appears in 2+ sources.
- Add to an existing page when the new source updates an existing concept.
- Do not create pages for passing mentions.
- Split pages over ~200 lines.
- If a new source contradicts an older page, keep both claims with dates and mark `contested: true`.

## Current operating rule
This wiki is the external knowledge layer. Hermes memory remembers Dimitris and durable preferences; this wiki stores broader project/world/context knowledge and source-backed notes.
