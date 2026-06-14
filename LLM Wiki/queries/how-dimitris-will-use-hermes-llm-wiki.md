---
title: How Dimitris Will Use Hermes LLM Wiki
created: 2026-06-14
updated: 2026-06-14
type: query
tags: [workflow, obsidian, llm-wiki, hermes]
sources: [raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md]
confidence: medium
---

# How Dimitris Will Use Hermes LLM Wiki

## Main workflow

Dimitris can send Hermes a source and say:

```text
ingest this into my wiki
```

or in Greeklish:

```text
vale auto sto wiki mou kai sindese to me ta iparxonta notes
```

Hermes should then:

1. Save the raw material in `LLM Wiki/raw/`.
2. Search existing pages for related concepts/entities.
3. Create/update notes under `concepts/`, `entities/`, `comparisons/`, or `queries/`.
4. Update `LLM Wiki/index.md`.
5. Append an entry to `LLM Wiki/log.md`.
6. If the vault is clean, commit/push to GitHub so the PC Obsidian can pull it.

## First source added

The first source is the YouTube transcript:

- `raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md`

## Related

- [[concepts/llm-wiki]]
- [[concepts/self-improving-knowledge-base]]
- [[entities/hermes-agent]]
