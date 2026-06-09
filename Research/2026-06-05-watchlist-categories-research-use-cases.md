# Watchlist categories and research use cases

Kanban: `t_fbf4d9e0`
Date: 2026-06-05 07:44 UTC
Status: Completed

## Συμπέρασμα

- MVP should ship with five fixed starter watchlists: `AI`, `Hermes`, `UX`, `ETFs`, `BJJ`.
- Users should be able to mute/hide or reorder these in MVP, but not create arbitrary new categories until the data model and notification rules are proven.
- Each watchlist should combine: saved notes from Obsidian, source feeds/search targets, cadence, and a small relevance rule-set.
- Watchlists should surface notes as actionable cards, not as raw feed dumps: title, why it matters, source, freshness, tags/entities, confidence/status, and next action.
- Later scope: custom categories, per-category agents/prompts, source weighting, alerts, user feedback training, and cross-watchlist deduplication.

## Product intent

A watchlist is a standing research lane for Dimitris. It answers:

1. What topic am I tracking?
2. Why does it matter to me?
3. Which sources should Solomon monitor or search first?
4. How often should it refresh?
5. Which existing notes/research artifacts are relevant?
6. What deserves attention now?

The Research tab should not become a generic RSS reader. The goal is curated, decision-oriented monitoring tied to Dimitris's personal vault and agent team.

## Data model recommendation

Minimum fields per watchlist:

```json
{
  "id": "ai",
  "label": "AI",
  "description": "Frontier AI, agents, local LLMs, tooling, papers, product shifts.",
  "enabled": true,
  "sort_order": 10,
  "cadence": "daily",
  "source_policy": "curated_sources_plus_manual_search",
  "note_query": ["#ai", "#llm", "#agents", "AI", "LLM", "Hermes"],
  "example_items": ["OpenAI model/API changes", "agent frameworks", "local LLM benchmarks"],
  "mvp_sources": ["official blogs/docs", "arXiv/papers", "GitHub repos", "trusted technical blogs"],
  "alert_threshold": "only if high relevance or breaking change",
  "owner_agent": "solomon"
}
```

Optional later fields:

- `custom_prompt`: category-specific Solomon instruction.
- `source_weights`: official docs > primary papers > source repos > trusted analysts > social.
- `blocked_sources`: noisy sources to suppress.
- `entities`: tracked products, tickers, people, competitors.
- `notification_policy`: daily digest, weekly digest, urgent-only, silent.
- `feedback`: user marked useful/not useful.

## Watchlist 1: AI

### User goal

Stay ahead of high-signal AI developments that affect Dimitris's products, automation stack, research workflow, and build priorities.

### Example items

- Frontier model releases, pricing/context/API changes.
- Agent frameworks and orchestration patterns.
- Local LLM hardware/software improvements.
- Important papers with practical implications.
- Developer tools: evals, tracing, retrieval, browser automation, voice, multimodal.
- Market/product shifts from OpenAI, Anthropic, Google DeepMind, Meta, xAI, Mistral, Nous, Hugging Face.

### Expected sources

MVP source classes:

- Official company blogs, changelogs, docs, and pricing pages.
- arXiv / Papers With Code / model cards.
- GitHub repositories and releases.
- High-signal technical blogs/newsletters.
- Existing Obsidian notes tagged `#ai`, `#llm`, `#agents`, `#local-llm`, `#research`.

Later sources:

- Conference proceedings.
- Benchmarks/eval leaderboards with source-quality scoring.
- Social/X/Reddit/HN only after dedupe and noise controls.

### Update cadence

- MVP: daily digest on active days; manual refresh from Research tab.
- Urgent exception: major model/API/pricing/security change can surface same day.
- Later: per-source cadence; weekly synthesis for trends.

### How notes should surface

- Card title: concise development or note title.
- `Why it matters`: direct implication for Dimitris.
- Related vault notes: recent AI research notes and project notes.
- Status: `new`, `updated`, `needs review`, `archived`.
- Confidence: high for official docs/papers; lower for social/rumors.
- Suggested action: read, test, create Petros task, ignore.

### MVP vs later

MVP:

- Fixed `AI` category.
- Manual/cron-generated note cards from Solomon research log and curated searches.
- Show latest 5–10 relevant notes.

Later:

- User-defined tracked entities, custom AI source lists, automatic GitHub release watching, model/pricing diff alerts.

## Watchlist 2: Hermes

### User goal

Track Hermes Agent, Dimitris's multi-agent setup, operational risks, implementation specs, and internal agent-team work.

### Example items

- Hermes Agent docs/features/config changes.
- Active kanban specs that affect dashboard, Research tab, voice, Telegram, scheduling, tools, skills.
- Agent team handoffs from Solomon/Petros/Ioudas/Samson/Panagia.
- Reliability issues: failed cron jobs, stale tasks, gateway problems, tool regressions.
- New skills or workflow conventions.

### Expected sources

MVP source classes:

- Hermes docs and local repo/config when available.
- Kanban board task handoffs and run metadata.
- Obsidian notes: `RESEARCH-LOG.md`, `IT-LOGS.md`, `Agent Logs/`, `Research/` Hermes-related notes.
- Local skills under the active Hermes profile.

Later sources:

- GitHub releases/issues for Hermes Agent if repository source is integrated.
- Service health/status checks.
- Cross-agent weekly summaries.

### Update cadence

- MVP: daily dashboard refresh gated by daily task flow; manual refresh.
- Operational exceptions: show errors/stale states immediately, but avoid noise.
- Later: hourly health summaries for background services if user opts in.

### How notes should surface

- Highlight specs, blockers, failed jobs, and review-required items.
- Group by `Implementation`, `Ops`, `Research`, `Decisions`.
- Link to the exact Obsidian note, kanban task id, and artifact path when available.
- Show a compact state: `done`, `blocked`, `needs review`, `running`, `stale`.

### MVP vs later

MVP:

- Fixed `Hermes` category.
- Pull from Research log and kanban task handoffs.
- No broad external monitoring unless a task explicitly requests it.

Later:

- Health dashboard integration, repository release watching, automatic follow-up task suggestions.

## Watchlist 3: UX

### User goal

Collect practical UX/product ideas for Dimitris's dashboard, agent interfaces, research cards, scheduled jobs UI, voice/Telegram flows, and future apps.

### Example items

- Dashboard patterns: cards, filters, status indicators, timelines.
- Research UX: citations, source confidence, watchlists, saved queries.
- Agent UX: handoffs, task review, error visibility, approvals.
- Voice/chat interaction patterns.
- Accessibility and mobile-first improvements.

### Expected sources

MVP source classes:

- Internal design notes and project specs.
- Product teardown notes written by Solomon.
- Public design-system docs and UX case studies when specifically researched.
- Existing vault folders: `Design/`, `Projects/`, `Research/`.

Later sources:

- Screenshot/image libraries if approved.
- Competitor monitoring.
- User analytics/session recordings if a product exists and privacy rules are defined.

### Update cadence

- MVP: weekly or manual; UX does not need daily noise.
- Triggered refresh when a dashboard/interface task is active.
- Later: sprint-based synthesis.

### How notes should surface

- Show pattern name, screenshot/link if available, where it could apply, and implementation effort.
- Relate notes to active UI components: Research tab, cron jobs, unified search, Telegram voice, dashboard cards.
- Mark as `inspiration`, `decision`, `spec`, or `anti-pattern`.

### MVP vs later

MVP:

- Fixed `UX` category.
- Search/link existing Design/Research/Projects notes.
- Manual Solomon summaries for relevant UI tasks.

Later:

- Structured pattern library, image thumbnails, competitor tracking, feedback loops from actual users.

## Watchlist 4: ETFs

### User goal

Track ETF-related research inputs without turning the dashboard into trading advice. Focus on long-term education, product comparisons, cost/tax considerations, and user-requested tickers.

### Example items

- ETF tickers Dimitris explicitly follows.
- Expense ratios, domicile, accumulating/distributing, UCITS availability.
- Broad market/sector exposure changes.
- Broker/platform constraints relevant to Greece/EU.
- Macro notes only if they affect the ETF research question.

### Expected sources

MVP source classes:

- Official issuer pages/factsheets/KIIDs.
- Exchange pages and regulator/issuer documents.
- User-provided tickers/watch items.
- Existing vault notes tagged `#finance`, `#etf`, `#investing`.

Later sources:

- Portfolio tracker integrations, price feeds, broker APIs, tax notes after scope approval.

### Update cadence

- MVP: weekly or manual; daily prices are not needed for research notes.
- Triggered refresh when Dimitris asks about a ticker or product comparison.
- Later: monthly factsheet diffing; optional price/holdings alerts.

### How notes should surface

- Strong disclaimers: research only, not financial advice.
- Show source date, issuer, domicile, TER/fees, distribution policy, currency, risks, and open questions.
- Link to source PDFs/factsheets and existing finance notes.
- Flag stale data after 30–90 days depending on field.

### MVP vs later

MVP:

- Fixed `ETFs` category but empty until Dimitris adds explicit tickers or notes exist.
- Manual search and note linking only.
- No buy/sell recommendations or automated trading signals.

Later:

- Custom ticker lists, factsheet diffing, broker availability, portfolio-aware analysis if user authorizes it.

## Watchlist 5: BJJ

### User goal

Track Brazilian Jiu-Jitsu learning, training notes, techniques, injury-prevention research, gyms/events, and personal progress references.

### Example items

- Techniques: guard retention, passing, escapes, submissions.
- Training plans and class notes.
- Competition/event notes.
- Injury prevention and mobility.
- High-quality instructionals or athlete breakdowns.

### Expected sources

MVP source classes:

- Dimitris's personal training notes in Obsidian.
- YouTube/transcript summaries only when user requests or a curated channel list exists.
- Trusted coach/gym pages.
- Sports medicine sources for injury-related topics.

Later sources:

- Calendar/gym schedule integrations.
- Wearable/training logs if approved.
- Curated YouTube channel monitoring.

### Update cadence

- MVP: weekly/manual; refresh after new training notes are added.
- Triggered refresh for specific technique questions.
- Later: training-cycle summaries and event reminders.

### How notes should surface

- Group by technique, position, problem, and next drill.
- Connect YouTube/video notes to personal practice notes.
- Mark injury/medical content as informational and recommend professional advice for medical decisions.
- Suggested action: drill, review video, ask coach, archive.

### MVP vs later

MVP:

- Fixed `BJJ` category.
- Search/link existing vault notes and manually generated video summaries.
- No automated scraping of YouTube/social feeds yet.

Later:

- Technique graph, training journal timeline, recurring drills, curated video feed monitoring.

## Category customization recommendation

### MVP

- Initial set is fixed: AI, Hermes, UX, ETFs, BJJ.
- User can:
  - enable/disable a category;
  - reorder categories;
  - set cadence among allowed values: manual, daily, weekly;
  - add simple tracked terms/tickers/entities within existing categories;
  - dismiss or archive surfaced notes.
- User cannot create/delete arbitrary top-level categories in MVP.

Rationale: fixed categories keep the UI simple and avoid creating a source/routing problem before the relevance model is validated.

### Later scope

Allow custom categories when all are true:

1. MVP shows that watchlists are used.
2. Source/noise controls exist.
3. Notes have stable tags/entities/status.
4. Notification preferences are implemented.
5. There is a migration path from fixed to custom categories.

Later custom category fields:

- label, description, icon/color;
- linked tags/folders;
- source list;
- cadence;
- prompt/instructions;
- alert policy;
- owner agent;
- privacy/sensitivity level.

## Surfacing logic

Recommended scoring inputs:

- Recency: newer notes higher, unless evergreen/pinned.
- Source authority: official docs, primary papers, issuer docs, local task handoffs weighted higher.
- User relevance: matches tracked terms/entities and active projects.
- Novelty: suppress duplicates and already-dismissed notes.
- Actionability: cards with clear next action rank higher.
- Freshness risk: stale finance/product data gets flagged rather than hidden.

MVP card fields:

```json
{
  "watchlist_id": "ai",
  "title": "OpenAI API pricing changed",
  "summary": "Short factual summary.",
  "why_it_matters": "Potential impact on Hermes costs or product design.",
  "source_title": "Official changelog",
  "source_url": "https://...",
  "note_path": "Research/2026-..",
  "status": "new",
  "confidence": "high",
  "updated_at": "2026-06-05T07:44:00Z",
  "suggested_action": "Review cost assumptions"
}
```

## MVP implementation checklist

1. Add watchlist definitions as static config or seed data.
2. Tag/link existing Research notes by watchlist where obvious.
3. Render category filter chips/cards in Research tab.
4. Show latest relevant notes from `RESEARCH-LOG.md` + `Research/` notes.
5. Support enable/disable, reorder, and manual refresh.
6. Store user tracked terms within category, not custom categories yet.
7. Keep alerts conservative: only high-relevance items and operational errors.

## Later-scope backlog

- Custom watchlist CRUD.
- Per-watchlist prompts and source lists.
- RSS/feed/source integrations.
- GitHub release watching.
- ETF factsheet diffing.
- YouTube transcript monitor for BJJ/UX.
- Feedback-based relevance training.
- Cross-watchlist dedupe.
- Watchlist analytics: useful/dismissed/opened.

## Assumptions / gaps

- This spec assumes the Research tab already has access to `RESEARCH-LOG.md` and Research notes.
- Exact frontend storage/API shape is not defined here; this is a product/research spec, not an implementation contract.
- ETF automation should remain research-only until Dimitris explicitly approves financial workflow scope.
- BJJ automated video monitoring should wait until Dimitris names trusted channels/sources.
- Hermes watchlist should prefer internal kanban/vault data over broad web monitoring.

## Τι σημαίνει για τον Δημήτρη

Start with useful defaults that match Dimitris's actual interests, but do not overbuild customization. The immediate value is a Research tab that tells him: "what changed, why should I care, and which note/task should I open?" Custom watchlists and automated feeds are worth adding only after the fixed five categories prove signal quality.
