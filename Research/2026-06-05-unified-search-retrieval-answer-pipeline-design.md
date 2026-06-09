# Unified Search / Ask my OS — Retrieval and Answer Pipeline Design

Date: 2026-06-05 01:44 UTC
Status: design ready
Kanban: `t_96b388e2`
Artifact: `/root/.hermes/kanban/workspaces/t_96b388e2/retrieval-answer-pipeline-design.md`

## Σύνοψη

Designed a local-first MVP pipeline for “Ask my OS”: explicit source registry, one-shot ingestion, SQLite FTS5 index, lexical ranking, evidence snippets, stable citations, and concise Greek evidence-only answers. First implementation slice should start with Obsidian markdown + Kanban DB, then add Hermes session DB once the schema is confirmed.

## Key decisions

- Use SQLite + FTS5 for MVP; avoid hosted/vector infrastructure until lexical validation proves it is insufficient.
- Use explicit allowlist sources only; no full-disk crawling.
- MVP source adapters: Obsidian files and Kanban DB first; session DB as slice 2 if schema validation is quick/safe.
- Every substantive answer bullet needs a citation: `file:...#Lx-Ly`, `kanban:t_id#run=...`, or later `session:<profile>:<session_id>#message=<id>`.
- Default answer format is concise Greek: 3–7 bullets, sources, and gaps/confidence.
- Sensitive paths and raw email/secrets/tokens remain excluded.
- Implementation remains gated until Dimitris explicitly approves starting.

## Note

See the workspace artifact for full components, data flow, schema sketch, ranking model, failure modes, privacy rules, and implementation task breakdown.
