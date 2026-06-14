---
title: Hermes Memory vs LLM Wiki
created: 2026-06-14
updated: 2026-06-14
type: comparison
tags: [hermes, memory, llm-wiki, knowledge-base]
sources: [raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md]
confidence: medium
---

# Hermes Memory vs LLM Wiki

## Short version

- [[concepts/hermes-memory|Hermes Memory]]: remembers Dimitris and stable personal/setup facts.
- [[concepts/llm-wiki|LLM Wiki]]: stores source-backed knowledge from Dimitris's world.

## Division of labor

### Hermes Memory

Use for durable facts that should automatically shape future conversations:

- User preferences
- Stable environment facts
- Recurring workflows
- Important long-term constraints

### LLM Wiki

Use for larger or source-backed knowledge:

- YouTube transcripts
- Research notes
- Meeting notes
- Project documentation
- Strategy docs
- Comparisons and query writeups

## Practical rule

If a fact is short, stable, and user-specific, it can go into Hermes Memory. If it is a document, source, transcript, research topic, or evolving project knowledge, put it in the LLM Wiki.

## Related

- [[entities/hermes-agent]]
- [[concepts/self-improving-knowledge-base]]
