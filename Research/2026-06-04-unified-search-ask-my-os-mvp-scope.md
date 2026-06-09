# Unified Search / Ask my OS — MVP Scope

Date: 2026-06-04 23:54 UTC
Status: final scope gate
Confidence: medium-high
Kanban: `t_b2fc474e`
Artifact: `/root/.hermes/kanban/workspaces/t_b2fc474e/unified-search-ask-my-os-mvp-scope.md`

## Συμπέρασμα
- The smallest useful MVP is local-first read-only Q&A/search over Obsidian notes plus at least one local agent-context source: session DB or kanban task history.
- Implementation must not start until Dimitris explicitly approves the start timing.
- MVP answers should be concise Greek by default, cite source links for substantive claims, and admit weak/no evidence instead of hallucinating.
- Raw email, finance/orders, secrets, full-disk crawling, external indexing, continuous watchers, and UI/voice integrations are out of scope until later.

## Facts
- Root backlog item `t_918df9ba` says: initial MVP can search local files/session DB, return concise Greek answers with source links, and must wait for Dimitris to pick when to start.
- Scope task `t_b2fc474e` requires a written distinction between MVP vs later backlog, supported initial sources, out-of-scope sources, Greek style, source-link requirements, privacy constraints, and success criteria.
- Downstream implementation task `t_363d2a80` is explicitly gated: “Implement the first working MVP after Dimitris explicitly approves starting.”

## MVP definition
- User asks one Greek/English natural-language question via one documented local command or internal endpoint.
- System retrieves snippets from allowlisted local sources.
- System answers in Greek with 3–7 concise bullets, source links, and explicit gaps/confidence when evidence is weak.
- Minimum sources: Obsidian notes plus session DB or kanban history if available.

## Out of scope for MVP
- Gmail/raw email, orders, receipts, expenses, investments, browser history, cookies, secrets, full-disk crawling, hosted vector DBs, continuous background watchers, voice, mobile/dashboard UI integration, and action-taking flows.

## Assumptions / Gaps
- Exact filesystem/database inventory is handled by `t_3cb6fc8b`; this scope note intentionally avoids implementation details.
- Retrieval/index design is handled by `t_96b388e2`.
- No explicit Dimitris start approval is present in the task context reviewed here.

## Εκτίμηση
This MVP should stay deliberately boring: local allowlist, read-only retrieval, citations, and Greek summaries. The main risk is scope creep into “search everything about me,” especially email/finance/secrets. The start gate must be enforced as a blocker, not treated as a soft preference.

## Suggested next steps
- `t_3cb6fc8b`: finish source inventory with exact paths/link formats.
- `t_96b388e2`: design the minimal retrieval/answer pipeline from this scope plus the inventory.
- `t_363d2a80`: block unless Dimitris gives explicit start approval.
- `t_1eaa2ead`: validate with at least 10 Greek queries after implementation exists.

## Sources
- Kanban task `t_918df9ba`: Agent OS backlog root.
- Kanban task `t_b2fc474e`: MVP scope/start-gate task.
- Kanban task `t_363d2a80`: implementation task requiring explicit start approval.

## Τι σημαίνει για τον Δημήτρη
Dimitris gets a safe, narrow first version of “Ask my OS” only when he decides to start, without accidental ingestion of sensitive data or premature UI/automation complexity.
