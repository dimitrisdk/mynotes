# `ρώτα τον Solomon` — Search / Q&A Interaction Spec

Date: 2026-06-05 07:43 UTC
Status: completed spec
Kanban: `t_52d2890f`
Artifact: `/root/.hermes/kanban/workspaces/t_52d2890f/solomon-search-interaction-spec.md`
Related: [[Research/2026-06-04-unified-search-ask-my-os-mvp-scope]], [[Research/2026-06-05-unified-search-retrieval-answer-pipeline-design]]

## Συμπέρασμα

- `ρώτα τον Solomon` should be a staged Q&A-over-sources experience, not only keyword search.
- MVP should answer over allowlisted Solomon research material: `RESEARCH-LOG.md` + `Research/*.md` first.
- It must answer in Greek by default, cite exact sources, and say when evidence is missing.
- Broader Ask-my-OS sources such as kanban/session DB are later slices unless already safely wired.

## MVP interaction flow

1. The user sees the search/ask box in the Researches tab after the relevant daily task is selected.
2. Accessible label / placeholder: exactly `ρώτα τον Solomon`.
3. User enters a Greek or English natural-language question.
4. Backend performs local read-only retrieval from allowlisted research sources.
5. Evidence is classified as `strong`, `partial`, or `none`.
6. Solomon-style answer is generated only from retrieved snippets.
7. UI renders:
   - `Απάντηση` — 3–7 concise bullets.
   - `Πηγές` — note/log citations.
   - `Κενά / αβεβαιότητες` when needed.
   - `Σχετικά αποτελέσματα` for related notes.
   - Confidence label: `Υψηλή`, `Μέτρια`, or `Χαμηλή`.

## Expected inputs

- Natural-language question in Greek or English.
- Optional future filters: topic/watchlist, status, date range, source type.
- Empty input should not run retrieval; show examples and recent research cards.
- Prompt injection or requests for secrets are treated as unsafe/unsupported, not instructions.

## Expected outputs and citations

MVP citation formats:

- Obsidian note: `[[Research/note-name]]` and, when possible, `/root/Documents/ObsidianVault/Research/foo.md#Lx-Ly`.
- Research log: `/root/Documents/ObsidianVault/RESEARCH-LOG.md#Lx`.
- Later kanban: `kanban:t_id#run=<id>`.
- Later session DB: `session:<profile>:<session_id>#message=<id>`.

Substantive claims without citations must be labeled as inference or omitted.

## Empty / weak states

- Empty prompt: `Ρώτα κάτι συγκεκριμένο.` plus examples.
- No evidence: `Δεν βρήκα αξιόπιστη πηγή. Δεν θα μαντέψω.` plus keyword retry / future create-research-task action.
- Partial evidence: show only supported bullets and list missing pieces.

## Safety limits

- Read-only MVP: no edits, task creation, shell commands, messages, or autonomous actions.
- Exclude secrets, `.env`, credentials, raw private logs, browser cookies, SSH keys, raw Gmail/email.
- Do not answer project/current claims from model memory.
- Do not fabricate citations.
- Financial/medical/legal topics are informational summaries only unless supported by existing sources.

## Technical / product dependencies

Required MVP dependencies:

- Source registry for `/root/Documents/ObsidianVault/RESEARCH-LOG.md` and `/root/Documents/ObsidianVault/Research/*.md`.
- Local SQLite FTS5/lexical index.
- Metadata extraction: title, path, line ranges, modified time, status, tags/source count where available.
- Backend endpoint such as `POST /api/research/ask-solomon`.
- Response contract with answer, confidence, citations, related results, gaps, retrieval mode.
- Frontend Researches tab integration with exact label `ρώτα τον Solomon`.
- Empty/partial/no-evidence UI states.
- Safe telemetry: source count, confidence, latency; no secret/raw-private logging.

Later dependencies:

- Kanban completed task adapter.
- Hermes session DB adapter with stable citations.
- Semantic embeddings if lexical MVP is insufficient.
- Watchlist filters: AI, Hermes, UX, ETFs, BJJ.
- Telegram/CLI parity.
- Confirmed follow-up research-task creation action.

## Τι σημαίνει για τον Δημήτρη

Dimitris gets an ask-style research interface that feels like Solomon but stays grounded: it cites the notes it used, works in Greek by default, and refuses to invent answers when the vault has no evidence.
