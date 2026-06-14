---
title: Hermes Agent
created: 2026-06-14
updated: 2026-06-14
type: entity
tags: [hermes, automation, vps, local-ai]
sources: [raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md]
confidence: medium
---

# Hermes Agent

Hermes Agent is Dimitris's central AI assistant running from the VPS and connected to Telegram, local files, tools, cron jobs, skills, and the Obsidian vault.

In the LLM Wiki workflow, Hermes is the agent that can ingest sources, maintain linked markdown pages, and answer questions using both [[concepts/hermes-memory|Hermes Memory]] and the [[concepts/llm-wiki|LLM Wiki]].

## Dimitris setup context

- VPS hosts the central Hermes instance.
- Windows PC can provide local model inference through Ollama over Tailscale.
- Obsidian vault on the VPS syncs through GitHub so notes are available on the PC.

## Related

- [[concepts/hermes-memory]]
- [[concepts/llm-wiki]]
- [[concepts/self-improving-knowledge-base]]
