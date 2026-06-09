---
title: Agent Logs Weekly Index
domain: agent-logs
subdomain: weekly
status: active
created: 2026-06-05
created_by: Petros
tags: [agent-logs, weekly, index]
---

# Agent Logs / Weekly Index

Weekly summaries (ISO week format: YYYY-Www).

## Template
- `[[_template-agent-weekly-summary]]` — reusable template

## Planned structure
- `YYYY-Www-agent-summary.md` — clean weekly summary
- `YYYY-Www-processed-sources.md` — processing manifest/index

## Cadence
- Run weekly: Monday 07:30 Europe/Athens
- Coverage: previous Monday 00:00 – Sunday 23:59 Europe/Athens
- Raw logs retained unchanged; only summaries written here

## Source folders (per workflow spec)
- `Agent Logs/*.md` (excluding Weekly/, Indexes/)
- `IT-LOGS.md`, `STRATEGY-LOG.md`, `RESEARCH-LOG.md`, `Dashboard Refresh Plan.md`

## Rules
- Do not delete raw logs
- Do not merge raw logs
- Do not rewrite raw log entries to mark them processed
- Store processing state only in separate index/manifest notes