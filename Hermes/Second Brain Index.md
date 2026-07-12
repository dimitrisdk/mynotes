---
title: Hermes Second Brain Index
type: index
tags: [hermes, obsidian, second-brain, chat-archive]
---

# Hermes Second Brain Index

## Conversation archive
- `Hermes/Chat Logs/` — daily, append-only archives of Hermes user/assistant conversations.

### Recent logs
- [[Chat Logs/2026-07-06]]
- [[Chat Logs/2026-07-07]]
- [[Chat Logs/2026-07-08]]
- [[Chat Logs/2026-07-09]]
- [[Chat Logs/2026-07-10]]
- [[Chat Logs/2026-07-11]]
- [[Chat Logs/2026-07-12]]
- Exporter: `/root/.hermes/scripts/export_sessions_to_obsidian.py`
- State watermark: `/root/.hermes/state/obsidian-session-export.json` (kept outside this vault).

## What this index is for
The daily logs are the recoverable chronological record. This index stays intentionally small: it points to the archive and the operating policy; it does not automatically promote every chat into permanent memory.

## Durable notes
- [[Memory]] — only explicit, safe durable facts belong here.
- [[Operations/Hermes Agent OS + Obsidian]] — architecture, privacy boundary, recovery and verification procedure.

## Privacy boundary
The exporter includes only `user` and `assistant` message text. Tool output, system prompts, reasoning fields, credentials, tokens, cookies, private keys, and secret-like values are excluded or redacted before writing.
