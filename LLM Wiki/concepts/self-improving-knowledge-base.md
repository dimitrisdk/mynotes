---
title: Self-Improving Knowledge Base
created: 2026-06-14
updated: 2026-06-14
type: concept
tags: [knowledge-base, obsidian, automation, workflow]
sources: [raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md]
confidence: medium
---

# Self-Improving Knowledge Base

A self-improving knowledge base is a workflow where every new source updates the existing note network instead of creating isolated summaries.

When Dimitris gives Hermes a source, Hermes should:

1. Save the raw source under `raw/`.
2. Search existing notes for related entities/concepts.
3. Create or update pages in `concepts/`, `entities/`, `comparisons/`, or `queries/`.
4. Add `[[wikilinks]]` so related pages connect.
5. Update `index.md` and append to `log.md`.

This makes the vault compound over time: new sources strengthen old pages, reveal contradictions, and make future answers better.

## Related

- [[concepts/llm-wiki]]
- [[concepts/hermes-memory]]
- [[queries/how-dimitris-will-use-hermes-llm-wiki]]
