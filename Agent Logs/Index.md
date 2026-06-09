---
title: Agent Logs Index
domain: agent-logs
status: active
created: 2026-06-05
created_by: Petros
source_policy: link-only-no-deletion
tags: [agent-logs, index, vault-structure]
---

# Agent Logs Index

Canonical index for durable activity logs and summaries for all agents.

## Sub-folders
- [[Agent Logs/Agents/Index|Agents]] — per-agent logs (Panagia, Petros, Samson, Ioudas, Solomon, Bezalel)
- [[Agent Logs/System/Index|System]] — cleanup, deploy, sync operations
- [[Agent Logs/Weekly/Index|Weekly]] — weekly summaries (ISO week format)
- [[Agent Logs/News Briefs/Index|News Briefs]] — daily news briefs

## Current linked content
### Root
- `[[../IT-LOGS]]` — infrastructure operations log
- `[[../STRATEGY-LOG]]` — strategy decisions log
- `[[../RESEARCH-LOG]]` — research index (also feeds dashboard Researches)

### Agent Logs (existing)
- `[[./Panagia]]` — Panagia operational log
- `[[./obsidian-cleanup-2026-06-04]]` — cleanup manifest
- `[[./2026-06-04-news-brief]]` — news brief log
- `[[./News Briefs/2026-06-03]]` — daily news brief

### Weekly (existing)
- `[[./Weekly/_template-agent-weekly-summary]]` — weekly summary template

### Briefs (separate folder — decide consolidation)
- `[[../Briefs/]]` — morning/news briefs (user-facing?)
- `[[../News/Daily News Briefs/2026-06-05]]` — daily news briefs

## Migration rules
- **Do not rewrite** raw historical logs unless explicitly asked
- Raw logs remain immutable; summaries quote/link but do not replace
- Weekly summaries go to `Agent Logs/Weekly/`; processing manifests to `Agent Logs/Indexes/`
- Decide whether `Briefs/` remains user-facing output and `Agent Logs/` remains machine/operational history
- News briefs have three homes; consolidate only after identifying which jobs write each path