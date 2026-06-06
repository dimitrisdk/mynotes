# Hermes Completed Tasks — Full Registry

> **Last updated:** 2026-06-06  
> **Total tasks:** 89 done + 22 archived = 111 completed  
> **Scope:** All tasks with `status = 'done'` from Kanban DB  
> **Purpose:** Searchable reference — what was built, what it does, practical value, how to use it

---

## 🎯 Quick Index by Domain

| Domain | Tasks | Key Deliverables |
|--------|-------|------------------|
| **Dashboard & UI** | 18 | Overview refresh, News thumbnails, Wellness merge, Agents view, Money section, System map, Shortcuts |
| **Orders/Purchases Pipeline** | 9 | Gmail → Orders Kanban (6 stages), Auto-polling, Expense proposals, Manual controls, Dark/teal UI |
| **Agents Control Room** | 3 | Agent status screen (6 profiles), Model/gateway/task counts, Browser-verified |
| **Calendar MVP** | 3 | Read-only Google Calendar Today/Upcoming, Athens TZ, Category labels |
| **Workspace/Asset Browser** | 2 | Obsidian + Betsson-Wireframes index, Markdown/PDF/SVG preview, Search & filters |
| **Obsidian Weekly Summaries** | 6 | Weekly summary generator, Taxonomy audit, Guardrail checklist, Vault structure audit |
| **Research Tab (Solomon)** | 8 | Research tab UI, Watchlists (AI/Hermes/UX/ETFs/BJJ), "ρώτα τον Solomon" search, Content model |
| **Voice Commands Layer** | 9 | STT/TTS audit, Telegram voice ingestion, Intent detection, MVP backlog, Shortcuts design |
| **Wellness/Fitness Dashboard** | 6 | Meal/training CRUD, Macro estimator (no Anthropic), Samson↔Dashboard sync, Wellness.md alignment |
| **Finance/Expense Tracking** | 7 | Monthly expense/investment tracker, Dashboard Money section, Initial entries (Protein €99.90, ETF €1300) |
| **Scheduled Tasks/Cron UI** | 5 | Cron list with next run/last result, Errors-only alerts, Pause/resume, End-to-end verification |
| **Gmail Automation** | 5 | Email classification (Skroutz/Proteinmax/Wolt/OpenAI), Safe confirmation rules, Expense proposals |
| **Bezalel Design Studio** | 4 | Implementation contract, UX brief, Plumbing audit, Dashboard preview wiring |
| **Infrastructure/Observability** | 4 | Langfuse pilot, Firecrawl integration, Playwright smoke tests, Kanban trace tests |
| **Workspace Data Model** | 5 | Unified asset schema, REST API (list/get/search), OpenAPI spec, 10k+ assets <200ms |
| **Orders Backend API** | 2 | SQLite order store + REST CRUD, KanbanBoard React component (6 columns, dark/teal) |
| **Calendar/Reminders Research** | 5 | Calendar patterns research, Product requirements, Low-spam reminder rules, Screen wireframes |
| **Obsidian Taxonomy** | 4 | Vault audit, Weekly summary format, Deletion guardrails, Folder structure mapping |

---

## 📋 Detailed Task Registry

### 1. DASHBOARD & UI REFRESH (MVP)

#### `t_4645b3d4` — Dashboard Refresh MVP (samson)
- **What:** Complete dashboard overhaul — layout, cards, navigation, thumbnails, wellness merge
- **Does:** Adds Today/Agents/Money/Wellness/News/Kanban/System cards; merges Fitness+Training→Wellness; adds News images; shortcuts; system map; token usage investigation
- **Practical value:** Dashboard went from sparse/outdated → modern command center
- **Use:** Visit http://100.107.122.62:9120 — all new cards visible

#### `t_77a9a632` — Overview Layout Refresh (petros)
- **What:** Compact dark mobile-safe layout with 7 top cards
- **Does:** Renders Today, Agents, Money, Wellness, News, Kanban, System cards in priority order
- **Practical value:** At-a-glance system health + quick navigation
- **Use:** Dashboard Overview page

#### `t_8316d391` — Full Agent Status Coverage (panagia)
- **What:** Agents overview includes all 6 profiles (panagia, petros, samson, ioudas, solomon, bezalel)
- **Does:** Shows gateway state, Telegram connected/running, model when available
- **Practical value:** See which agents are live, their models, task counts
- **Use:** Dashboard → Agents card / Agents page

#### `t_76039393` — News Image Thumbnails (solomon)
- **What:** Article images from RSS/enclosure/og:image with fallback
- **Does:** Thumbnails in News list + top 3 on Overview; gradient placeholder for missing
- **Practical value:** Visual scanning of news, faster triage
- **Use:** Dashboard → News tab / Overview top articles

#### `t_c6d902eb` — Merge Fitness+Training→Wellness (petros)
- **What:** Single Wellness section combining food/macros, weight, meal photos, training streaks/logs
- **Does:** Sidebar label "Wellness"; old routes redirect; unified page with sub-tabs
- **Practical value:** One place for all health data
- **Use:** Dashboard → Wellness tab

#### `t_10a4f346` — Shortcuts, System Map, Usage Data (samson)
- **What:** Quick links: Money, Wellness, Kanban, Obsidian/TODO, HA All Off, Telegram agents, News refresh, GitHub, backup status + system architecture SVG
- **Does:** Safe actions as links; system map renders; token usage shows "not available" honestly
- **Practical value:** One-click access to common actions; visual system understanding
- **Use:** Dashboard Overview cards

#### `t_8aefad13` — Fix Production Money Route (samson)
- **What:** `/api/money` was 404 in production
- **Does:** Restored route/deployment wiring; seed data visible
- **Practical value:** Money section works end-to-end
- **Use:** Dashboard → Money tab / Overview Money card

#### `t_5a447b1b` — Build, Tests, Browser Verification (default)
- **What:** Client build + server tests + browser verify all MVP changes
- **Does:** Confirmed Overview, News, Wellness, Money, Agents, System/diagram cards functional
- **Practical value:** Production-ready deployment
- **Use:** `npm run build` + `npm run test` + manual browser check

---

### 2. ORDERS / PURCHASES PIPELINE (Gmail → Kanban)

#### `t_f2b9b2d6` — Orders Kanban End-to-End Pipeline (ioudas)
- **What:** Consolidated full pipeline: Gmail scanning → classification → Orders Kanban (6 stages)
- **Does:** 
  - Gmail classifier extracts merchant, amount, order_id, stage
  - 6 Kanban stages: to_buy → ordered_processing → shipped → ready_for_pickup → delivered/picked_up → return_refund_problem
  - Auto-update from shipping emails (tracking/shipped/delivered)
  - Expense proposals from receipts/subscriptions (read-only, confirmation-gated)
  - Manual order entry + stage update controls
  - Dark/teal Kanban UI with horizontal scroll, compact mode, assignee filter, search
- **Completed:** Proteinmax.gr order #247590 (€99.90) auto-detected; API endpoints GET/POST /api/orders, POST /api/email-automation/process
- **Practical value:** Zero-manual order tracking for Skroutz/Proteinmax/Wolt/OpenAI emails
- **Use:** Dashboard → Orders tab; email automation runs on cron

#### `t_00281b63` — Orders MVP: Gmail Polling Every N Minutes (ioudas)
- **What:** Scheduled read-only Gmail polling for dimitris.fb@gmail.com
- **Does:** Classifies order confirmation/shipping/ready_pickup/receipt/subscription; creates/updates Kanban only on high confidence; receipts→expense proposals; logs each run (processed/created/updated/skipped/errors)
- **Practical value:** Orders update automatically without manual forwarding
- **Use:** Cron job triggers polling; check Orders Kanban for updates

#### `t_6e863337` — Backend Order Store & REST API (panagia)
- **What:** SQLite order store with full CRUD endpoints
- **Does:** GET/POST/PATCH/DELETE /api/orders/:id; schema: id, vendor, amount, currency, stage (6 values), delivery_date, pickup_location, payment_status, notes, source, timestamps
- **Seeded:** 2 Skroutz orders (Birkenstock €89, Ernie Ball €17.70 — both Lidl Peristeri, ready_pickup, 2026-06-08)
- **Practical value:** Persistent, queryable order data with validation
- **Use:** `curl -X GET http://localhost:3001/api/orders` etc.

#### `t_b17a7225` — KanbanBoard React Component (panagia)
- **What:** Reusable 6-column Kanban board (dark/teal theme, responsive)
- **Does:** Columns: To buy, Ordered/processing, Shipped, Ready for pickup, Delivered, Return/refund/problem; stage title + count badge; onOrderMove callback; horizontal scroll desktop, stacks mobile
- **Practical value:** Clean, performant UI for order tracking
- **Use:** Import in `/orders` page; pass orders array + handler

#### `t_42bbf352` — Email Signals Inventory (solomon)
- **What:** Matrix of Gmail categories to detect: Skroutz orders, receipts, subscriptions, security alerts
- **Does:** Captures sender/domain patterns, subject/body cues, downstream action per category
- **Practical value:** Source-driven classifier design — no guessing
- **Use:** Reference for Gmail classifier implementation

#### `t_3c05e489` — Safe Confirmation Rules for Orders (samson)
- **What:** Policy for auto-updating Orders Kanban from email
- **Does:** Defines safe confirmation thresholds; what requires manual approval; uncertain match handling; default read-only
- **Practical value:** Prevents wrong order updates; trust-but-verify
- **Use:** Classifier checks rules before Kanban write

#### `t_a937c555` — Email-to-Expense Proposal Flow (samson)
- **What:** Receipts → proposed expenses (not auto-created)
- **Does:** Minimal data extraction; proposal surfacing for review; duplicate prevention
- **Practical value:** Captures expenses without mutation risk
- **Use:** Dashboard → Money/Expenses → Proposals review

#### `t_0d8aa954` — Gmail Detection & Classification Pipeline (bezalel)
- **What:** Email scanner reading Gmail → structured detections
- **Does:** Read-only by default; classifies using scenario matrix; preserves evidence per match
- **Practical value:** Reliable, auditable email parsing
- **Use:** Cron job triggers; output feeds Orders/Expense proposals

---

### 3. AGENTS CONTROL ROOM

#### `t_ce3bb39e` — Agents MVP: Status & Tasks per AI Assistant (petros)
- **What:** Simple screen/API showing all 6 profiles + what they're doing
- **Does:** Model/provider, gateway availability, running/ready/blocked task counts, latest 3 tasks; read-only; works with zero active tasks
- **Practical value:** Instant visibility into agent fleet health
- **Use:** Dashboard → Agents page; browser-verified

#### `t_5ef7c55d` — Map Control Room Users & Workflows (petros)
- **What:** Requirements for who uses Control Room and top workflows
- **Does:** Covers 6 agents; 5-8 workflows: status check, gateway check, last activity, assigned tasks, log review, quick routing; must-have fields per agent
- **Practical value:** Guides UI/backend scoping with confirmed vs unknown
- **Use:** Reference for future Control Room enhancements

#### `t_acbae261` — Audit Agent Status & Activity Data (solomon)
- **What:** Inventory of available data sources per agent field
- **Does:** Maps online/offline, gateway status, last activity, assigned tasks, recent logs, routing state → source systems (APIs, DB, Kanban, events)
- **Practical value:** Knows exactly where each dashboard field comes from
- **Use:** Implementation reference for Agents page data wiring

---

### 4. CALENDAR MVP

#### `t_e04a9e60` — Calendar MVP: Today View from Google Calendar (petros)
- **What:** Read-only Today + 7-day view from Panagia's Google Calendar
- **Does:** Reuses existing OAuth token; fetches today+7 days; normalizes title, start/end, location, source calendar, category (medical/food/training/renewal/other); Athens TZ; empty/error states
- **Practical value:** See appointments at a glance without leaving dashboard
- **Use:** Dashboard → Calendar page / Overview Today card

#### `t_39f68d0c` — Research Calendar & Reminder Patterns (solomon)
- **What:** Compared Google Cal, Apple Cal/Reminders, Todoist, Fantastical, Notion Cal
- **Does:** Source-backed summary: what works for daily review, low-spam reminders, read-only integrations
- **Practical value:** Evidence-based design decisions for calendar layer
- **Use:** Reference for reminder engine v2

#### `t_e41c55d8` — Calendar Layer Product Requirements (petros)
- **What:** Concrete requirements for dashboard calendar layer
- **Does:** Event categories (today timeline, medical, renewals, BJJ/yoga/training, general reminders); read-only default; approval-gated writes; 5+ user scenarios; non-goals
- **Practical value:** Clear spec for v1 implementation
- **Use:** Future calendar enhancement roadmap

#### `t_5b015b2c` — Low-Spam Reminder Rules (petros)
- **What:** Reminder behavior policy
- **Does:** Timing rules, escalation, quiet hours, category differentiation (medical/renewal vs training), snooze/dismiss/approve flows
- **Practical value:** Reminders that don't become noise
- **Use:** Reminder engine implementation reference

#### `t_2272cb7d` — Calendar Screen Wireframe (petros)
- **What:** UX structure for dashboard calendar layer
- **Does:** Today timeline, upcoming appointments, renewals/subscriptions, training events, pending approvals; glance info + actions; read-only vs editable visual distinction
- **Practical value:** Text wireframe ready for design/implementation
- **Use:** Bezalel/Design implementation reference

---

### 5. WORKSPACE / ASSET BROWSER

#### `t_14bff90a` — Workspace MVP: Browse Obsidian & Betsson Files (panagia)
- **What:** First Workspace browser for local files
- **Does:** Indexes /root/Documents/ObsidianVault + /root/Documents/Betsson-Wireframes; markdown, images, PDFs, SVG/HTML previews; list/grid + search by filename/path + type/source filters; open/download paths; graceful missing folder handling
- **Practical value:** Unified access to design assets + notes without leaving dashboard
- **Use:** Dashboard → Workspace tab

#### `t_4f17034a` — Workspace Schema: Unified Asset Model & API (panagia)
- **What:** Data model + REST endpoints for all artifact types
- **Does:** Schema: source, timestamps, tags, paths/URLs, preview thumbnails; API: list (filters: type, source, date), get single (signed preview URL), search by tag/filename; <200ms for 10k+ assets; OpenAPI spec
- **Practical value:** Performant, scalable asset browsing foundation
- **Use:** `/api/workspace/assets` endpoints; Workspace UI consumes this

---

### 6. OBSIDIAN WEEKLY SUMMARIES & TAXONOMY

#### `t_44860db0` — Obsidian MVP: Weekly Summary Without Restructuring (samson)
- **What:** Generates weekly summary note in Obsidian
- **Does:** Reads IT-LOGS, RESEARCH-LOG, STRATEGY-LOG, Wellness/TODO; creates `/Weekly Summaries/YYYY-WW.md` with wins, blockers, next actions, decisions needed; no structural changes/deletes/merges
- **Practical value:** Recurring high-signal summary without vault disruption
- **Use:** Run manually or cron → check Weekly Summaries folder

#### `t_83991243` — Audit Vault & Propose Obsidian Taxonomy (solomon)
- **What:** Reviewed vault for People, Finance, Health, Recipes, Research, Projects, Design, Agent Logs
- **Does:** Identifies folders, tags, naming patterns, duplicates, Keep Import categories; proposes structure preserving all approved categories
- **Practical value:** Clear migration path without data loss
- **Use:** Reference for future vault restructuring

#### `t_033bb9ab` — Audit Obsidian Folder Structure by Domain (solomon)
- **What:** Detailed inventory per domain (People, Finance, Health, Recipes, Research, Projects, Design, Agent Logs)
- **Does:** Current state + recommended changes + risks/ambiguities; no deletions without approval
- **Practical value:** Domain-level clarity for organization
- **Use:** Per-domain restructuring planning

#### `t_07835851` — Weekly Summary Format for Core Domains (solomon)
- **What:** Reusable weekly summary template for 8 domains
- **Does:** Sections, field definitions, example filled template; works with existing notes as-is
- **Practical value:** Consistent, scannable weekly reviews
- **Use:** Copy template → fill weekly

#### `t_56229a55` — Obsidian Guardrail Checklist Before Delete/Merge (samson)
- **What:** Operational checklist preventing accidental deletions/merges
- **Does:** Explicit approval required + recorded before structural changes
- **Practical value:** Vault safety net for automation
- **Use:** Attach to any restructuring plan/automation

---

### 7. RESEARCH TAB (SOLOMON)

#### `t_7baa6b3e` — Research Tab Data & Interaction Model (solomon)
- **What:** Spec for Solomon's Research tab
- **Does:** Recent notes, sources/links, draft/final status, search box "ρώτα τον Solomon", watchlists (AI/Hermes/UX/ETFs/BJJ); gated behind daily task selection
- **Practical value:** Clear implementation spec — no ambiguity
- **Use:** Frontend implementation reference

#### `t_40dc52c2` — Research Tab Layout & Sections (petros)
- **What:** UI shell for Research tab in Agent OS
- **Does:** All required sections + sensible empty states; gated behind daily task selection
- **Practical value:** Rendered tab ready for data wiring
- **Use:** Dashboard → Research tab (after daily task selected)

#### `t_c34457d3` — Research Tab Data Fetching & State (ioudas)
- **What:** Wires tab to load/manage data
- **Does:** Recent notes, sources, draft/final status, watchlist topics; state transitions (loading/empty/populated); refresh on daily task change
- **Practical value:** Live data in Research tab
- **Use:** Automatic on tab visit

#### `t_ffaea718` — Solomon Search: "ρώτα τον Solomon" (solomon)
- **What:** Search box behavior implementation
- **Does:** Query submission, results/guidance display, no-result/error handling
- **Practical value:** Natural language research queries
- **Use:** Type in Research tab search box → get contextual response

#### `t_301607a5` — Research Tab E2E Validation (samson)
- **What:** End-to-end verification after daily task selection
- **Does:** Watchlists render (AI/Hermes/UX/ETFs/BJJ), sources/links visible, draft/final status, tab hidden until selection
- **Practical value:** Confirmed working integration
- **Use:** QA checkpoint passed

#### `t_42bbf352` — (see Orders: Email Signals Inventory)

#### `t_8dc16b6f` — Bezalel Design Studio Implementation Contract (ioudas)
- **What:** UX + implementation plan for Design Studio
- **Does:** Wireframe/prototype generation flows, screenshot annotation, UX audits, export/preview, /root/Documents/Betsson-Wirefiles paths, dashboard preview entry points, data formats, naming conventions
- **Practical value:** Concrete checklist for downstream workers
- **Use:** Design Studio implementation roadmap

#### `t_463feff7` — Audit Bezalel Design Studio Plumbing (bezalel)
- **What:** Inspected current codebase, config, routes, storage, dashboard preview
- **Does:** Maps wireframe gen, annotation, audits, export, preview integration points; confirms Betsson-Wireframes path; identifies blockers
- **Practical value:** Know exactly what exists vs needs building
- **Use:** Pre-implementation audit

#### `t_1c91b462` — Research Tab UX & Navigation Brief (bezalel)
- **What:** Product-facing UX brief
- **Does:** Sections, navigation, list/detail behavior, status indicators, source presentation, search placement, watchlist access, loading/empty/error states, mobile/desktop
- **Practical value:** Design-ready wireframe
- **Use:** Bezalel design implementation

---

### 8. VOICE COMMANDS LAYER

#### `t_61d96c58` — Video Backlog: Voice Commands Layer (petros)
- **What:** Triage item from video discussion
- **Does:** Telegram voice → transcription → optional TTS → shortcuts (log expense, add reminder, send task to Petros, summarize video/link)
- **Practical value:** Scoped voice feature backlog
- **Use:** Reference for voice MVP implementation

#### `t_5a447b1b` — (see Dashboard: Build, Tests, Verification)

#### `t_5ba47002` — Audit STT/TTS & Telegram Voice Handling (solomon)
- **What:** Pre-implementation audit of voice pipeline
- **Does:** Telegram message receipt, voice download, STT providers, TTS providers, secrets/config; findings: files, env vars, capabilities, gaps, risks, smallest safe first path
- **Practical value:** Start coding without rediscovery
- **Use:** Voice implementation kickoff reference

#### `t_0aeeae97` — Telegram Voice Transcription Intake (solomon)
- **What:** First working slice: detect voice, download, STT, return transcript to Hermes
- **Does:** Config checks for missing STT; graceful failure; passes transcript onward (no actions yet)
- **Practical value:** Functional voice → text pipeline
- **Use:** Send voice to Telegram bot → get transcription

#### `t_6be850d3` — Map Voice Command Use Cases & MVP Boundaries (petros)
- **What:** Product/use-case map for voice layer
- **Does:** Per use case: intent, example utterances, expected output/action, integrations, failure modes, MVP/v1/later classification
- **Practical value:** Prioritized MVP with explicit non-goals
- **Use:** Voice feature scoping document

#### `t_4f36a080` — Research Telegram Voice, STT, TTS, Link Summary (solomon)
- **What:** Compared options for voice stack
- **Does:** Telegram bot voice handling, STT providers, TTS providers, video/link summarization; compared by API, reliability, latency, cost, language, privacy, complexity
- **Practical value:** Recommended MVP stack with trade-offs
- **Use:** Technology selection for voice implementation

#### `t_81c80dce` — Incremental Voice Implementation Backlog (petros)
- **What:** Phased backlog from scoped workflow
- **Does:** Milestones: Telegram ingestion → transcription → intent detection → first shortcuts → confirmation → response → observability → TTS/video-summary; dependencies + acceptance criteria
- **Practical value:** Actionable execution order
- **Use:** Sprint planning for voice features

#### `t_7f192f06` — Solomon Research Tab Content Model (solomon)
- **What:** What Research tab stores/displays
- **Does:** Recent notes, sources/links, draft vs final, tags/topics, timestamps, authorship, watchlist relations; facts/assumptions/open questions separated
- **Practical value:** Data model for Research tab persistence
- **Use:** Backend schema reference

#### `t_fbf4d9e0` — Watchlist Categories & Research Use Cases (solomon)
- **What:** Spec for 5 watchlists (AI, Hermes, UX, ETFs, BJJ)
- **Does:** Per watchlist: user goal, example items, sources, update cadence, surfacing logic; fixed vs extensible categories
- **Practical value:** Watchlist behavior defined
- **Use:** Research tab watchlist implementation

#### `t_52d2890f` — Triage "ρώτα τον Solomon" Search Experience (solomon)
- **What:** Greek search prompt → interaction spec
- **Does:** Keyword vs semantic vs Q&A; MVP staging; inputs/outputs; citation behavior; empty states; language expectations; safety limits
- **Practical value:** Clear search UX definition
- **Use:** Search component implementation

---

### 9. WELLNESS / FITNESS DASHBOARD

#### `t_6f5e85fe` — Dashboard Delete/Update Tools for Meals/Training (samson)
- **What:** CRUD for wrong entries (instead of adding corrections)
- **Does:** Delete/update meal/training entries via dashboard MCP/tools
- **Practical value:** Clean data without compensation entries
- **Use:** Dashboard → Wellness → edit/delete buttons

#### `t_d6e30ea3` — Macro Estimator: Switch to Hermes Provider (petros)
- **What:** Removed Anthropic API dependency for meal macro estimation
- **Does:** Uses existing Hermes model/provider abstraction; auto-estimate works without Anthropic creds
- **Practical value:** Zero external API cost for macro estimation
- **Use:** Log meal without macros → auto-filled via Hermes

#### `t_3ff04f93` — Samson ↔ Dashboard Sync for Food/Training (samson)
- **What:** Automatic sync + Obsidian Wellness.md alignment
- **Does:** Samson logs → dashboard automatically; shared source of truth
- **Practical value:** Single entry point, consistent everywhere
- **Use:** Log in Samson or Dashboard → both update

#### `t_f50bebd3` — Update/Delete Handlers for Meal/Training (ioudas)
- **What:** MCP/dashboard endpoints for corrections
- **Does:** Identify target, apply field updates, safe removal; matches dashboard conventions
- **Practical value:** Programmatic correction API
- **Use:** Dashboard tools or direct API calls

#### `t_2d2b1a29` — Correction Payloads & Validation Rules (petros)
- **What:** Request shapes + validation for update/delete
- **Does:** Required identifiers, reject invalid/partial, prevent corruption; documented fields/error cases
- **Practical value:** Safe, predictable correction API
- **Use:** Reference for tool definitions

#### `t_471b4050` — Persist Edits/Deletions Through Data Layer (panagia)
- **What:** Wire corrections to storage layer
- **Does:** Changes saved, visible in dashboard; deletes target only; atomic updates
- **Practical value:** Corrections actually persist
- **Use:** Automatic on dashboard edit/delete

#### `t_cfa93c3d` — Verify Correction Flows (default)
- **What:** E2E test: update, delete, invalid ID, malformed payload
- **Does:** Confirmed dashboard reflects corrected data; no compensating entries needed
- **Practical value:** Regression-free correction UX
- **Use:** QA checklist passed

#### `t_a9ebbac7` — Shared Wellness Data Contract (samson)
- **What:** Canonical schema for food/training across Samson, Dashboard, Obsidian
- **Does:** Required fields, timestamps, IDs, update semantics, conflict resolution, authoritative system per field
- **Practical value:** Unambiguous sync implementation
- **Use:** Sync logic reference

#### `t_b996c734` — Samson-to-Dashboard Log Sync (samson)
- **What:** Automatic propagation of new/updated logs to dashboard
- **Does:** Creates, updates, deduplication via stable IDs; idempotent; error surfacing
- **Practical value:** Reliable sync without duplicates
- **Use:** Automatic on Samson log

#### `t_7ff3ee5b` — Align Obsidian Wellness.md with Dashboard (ioudas)
- **What:** Wellness.md reflects dashboard as source of truth
- **Does:** Format matches canonical schema; manual edit merge/reject rules prevent drift
- **Practical value:** Vault stays consistent with dashboard
- **Use:** Edit in dashboard → Wellness.md updates

#### `t_d27567fa` — Verify E2E Sync Consistency (petros)
- **What:** Full flow test across Samson, Dashboard, Wellness.md
- **Does:** New entries, edits, duplicates converge to same state; edge cases documented
- **Practical value:** Proven shared source-of-truth
- **Use:** QA sign-off

---

### 10. FINANCE / EXPENSE TRACKING

#### `t_b0cdabd6` — Set Up Solomon Researcher Profile (petros)
- **What:** Created `solomon` profile (Research Analyst & Knowledge Scout)
- **Does:** Greek-first, source-driven, skeptical, concise; separates facts/assumptions/opinions; ends with "Τι σημαίνει για τον Δημήτρη"; gpt-5.5/openai-codex; Obsidian logging (RESEARCH-LOG.md + Research/); lean tool/skill access
- **Practical value:** Dedicated research specialist
- **Use:** `hermes --profile solomon chat -q "..."`

#### `t_981d66ff` — Monthly Expense & Investment Tracking (ioudas)
- **What:** Tracking structure for monthly expenses/investments
- **Does:** Initial entries: protein powder €99.90 (wellness/supplement), ETF €1300; monthly summary flow design
- **Practical value:** Financial visibility per month
- **Use:** Send expenses to Ioudas → monthly summary review

#### `t_8cbc0992` — Monthly Finance Tracking Structure (ioudas)
- **What:** Data structure with date, month, description, amount, currency, type, category, account, notes, source/status
- **Does:** Types: Expense, Investment; categories: wellness/supplement, ETF; ready for ongoing entries
- **Practical value:** Extensible finance tracker
- **Use:** Ioudas captures new expenses

#### `t_ca20fbd3` — Record Initial Expense/Investment Entries (ioudas)
- **What:** Seeded tracker with 2 entries
- **Does:** Protein €99.90 (expense, wellness/supplement), ETF €1300 (investment, ETF); marked as initial
- **Practical value:** Baseline data for dashboard
- **Use:** Dashboard Money section shows these

#### `t_42b75097` — Monthly Summary & Intake Flow (ioudas)
- **What:** Workflow for ongoing tracking
- **Does:** Capture/categorize/mark incoming expenses; monthly summary: total expenses, by category, total investments, notable changes, uncategorized items
- **Practical value:** Reviewable monthly financial snapshot
- **Use:** Monthly check-in with Ioudas

#### `t_d5195670` — MVP Dashboard Expenses/Money Section (samson)
- **What:** Dashboard card for monthly expenses/investments
- **Does:** Totals (expenses/investments), category split, entry list (date, amount, type, category, description, source); seeded data; lean read-only MVP
- **Practical value:** Financial visibility in dashboard
- **Use:** Dashboard → Money tab / Overview Money card

#### `t_048c0390` — Expenses/Money Data Contract (solomon)
- **What:** Minimal data shape for MVP Money section
- **Does:** Fields: date, amount, type, category, description, source/profile; monthly totals computation; CRUD vs read-only decision
- **Practical value:** Implementation-ready data contract
- **Use:** Backend/frontend alignment

#### `t_330f81f9` — Implement Dashboard Expenses/Money Section (samson)
- **What:** Main dashboard card "Expenses" or "Money"
- **Does:** Current-month totals, simple entry list, CRUD if data layer supports, otherwise read-only compatible
- **Practical value:** Live Money section
- **Use:** Dashboard → Money

#### `t_817c6446` — Seed Initial Expenses/Investments (petros)
- **What:** Dashboard seed data for 2026-06-03
- **Does:** €99.90 expense (supplements, protein powder), €1300 investment (ETF); source Ioudas/manual
- **Practical value:** Immediate dashboard visibility
- **Use:** Auto-populated on deploy

#### `t_063ab2fb` — Verify Dashboard Section & Totals (samson)
- **What:** Browser verification of Money section
- **Does:** Section visible, totals correct (expenses vs investments), list shows required fields
- **Practical value:** Confirmed working
- **Use:** QA passed

---

### 11. SCHEDULED TASKS / CRON UI

#### `t_f0b1cf43` — Cron Jobs List & Status UI Requirements (solomon)
- **What:** Spec for scheduled tasks dashboard
- **Does:** List jobs with next run, last result, enabled/paused, errors-only alerts; user interactions, empty/loading/error states; silent-success preference
- **Practical value:** Implementation-ready cron UI spec
- **Use:** Backend/frontend cron implementation

#### `t_4f0734de` — Implement Scheduled Tasks API & State (petros)
- **What:** Backend endpoints for cron jobs
- **Does:** GET /api/scheduled-jobs (list with next_run, last_result, enabled, last_error, metadata); RSS/ical sources if applicable; pause/resume toggle; response schema; auth
- **Practical value:** Functional cron API
- **Use:** Dashboard Cron tab consumes this

#### `t_2d83e8b3` — Build Cron Dashboard UI (panagia)
- **What:** Cron jobs list/table in dashboard
- **Does:** Columns: name, schedule, next run, last result, enabled/paused, last error; accordion for history; visual status badges; pause/resume if backend supports
- **Practical value:** Visual cron management
- **Use:** Dashboard → Scheduled Tasks tab

#### `t_b9645712` — Errors-Only Scheduled Job Alerts (solomon)
- **What:** Silent successes, actionable failure alerts
- **Does:** Successful maintenance/backup = no alert; failures = alert with job name, time, error summary, inspect link; deduplication for repeated failures
- **Practical value:** No notification spam; only actionable alerts
- **Use:** Automatic on cron failure

#### `t_7edd8ef4` — Verify Scheduled Tasks E2E (solomon)
- **What:** E2E test: healthy maintenance, backup, paused, failing jobs
- **Does:** Next run, last result, enabled/paused, latest error accurate; success silence confirmed; failure alerts fire; refresh + timezone formatting
- **Practical value:** Proven cron UI + alerting
- **Use:** QA sign-off

---

### 12. GMAIL AUTOMATION (Orders + Expenses)

*See Orders Pipeline (section 2) for core tasks. Additional:*

#### `t_01dbb6b4` — Ioudas Validation with Gmail Samples
- **What:** User-provided Gmail samples for classifier validation
- **Does:** Tests classifier against real Skroutz order, Skroutz ready, Proteinmax, Wolt receipt, OpenAI subscription
- **Practical value:** Proven accuracy on real emails
- **Use:** Dependency for Orders MVP cron

---

### 13. BEZALEL DESIGN STUDIO

*See Research Tab (section 7) for `t_8dc16b6f`, `t_463feff7`, `t_1c91b462`. Additional:*

#### `t_463feff7` — (see Research Tab: Audit Bezalel Design Studio Plumbing)

---

### 14. INFRASTRUCTURE / OBSERVABILITY

#### `t_64a6a24d` — Infra: Observability, Research Tools, Dashboard Tests (petros)
- **What:** Merged unblockers for Petros infrastructure
- **Does:** 
  - **Langfuse Pilot:** Instrument low-risk workflow (Solomon research/dashboard CLI); real traces (metadata, model, tools, timings, errors, tokens); Usage/LLM Observability card after real data; self-host vs cloud check; no secrets in logs; add LANGFUSE keys to Petros .env
  - **Firecrawl Research Workflow:** Safe helper for Solomon (hosted API); save summaries to Obsidian Research with tags/counts; avoid self-host; add FIRECRAWL_API_KEY to Solomon .env
  - **Playwright Smoke Tests:** Stabilize in CI/VPS; cover /researches + overview; screenshot on failure; real RESEARCH-LOG.md data; run `npm run test:e2e` on VPS
  - **Completed:** Researches tab QA+deploy; Kanban trace tests
- **Practical value:** Observability, research hardening, CI stability
- **Use:** `npm run test:e2e`; Langfuse dashboard; Firecrawl via Solomon

#### `t_ba96c5e1` — Langfuse Pilot (merged into t_64a6a24d)
#### `t_b7358c63` — Firecrawl Research Workflow (merged into t_64a6a24d)
#### `t_c7901e82` — Playwright Smoke Tests (merged into t_64a6a24d)
#### `t_e48f833c` — Researches Tab QA + Deploy (merged into t_64a6a24d, done)
#### `t_bd89fa51` — Test Kanban Trace Creation (petros)
- **What:** Verify trace entries on task create
- **Does:** Create task → trace log shows create event with timestamp, task ID, action 'create', metadata (assignee, column, priority) within 2s
- **Practical value:** Audit trail for Kanban actions
- **Use:** Automatic on task creation

#### `t_2d0b6b07` — Test Trace Creation - New Task (petros)
- **What:** Verify trace log entries created correctly
- **Practical value:** Regression test for trace system
- **Use:** Test suite

---

### 15. CALENDAR / REMINDERS RESEARCH

*See Calendar MVP (section 4) for `t_39f68d0c`, `t_e41c55d8`, `t_5b015b2c`, `t_2272cb7d`.*

---

### 16. OBSIDIAN TAXONOMY

*See Obsidian Weekly Summaries (section 6) for `t_83991243`, `t_033bb9ab`, `t_07835851`, `t_56229a55`.*

---

### 17. RESEARCH / TEST QUERIES (Solomon)

#### `t_2859a764`, `t_1d2e0122`, `t_98a193b9`, `t_3aeee46f`, `t_1c56899f`, `t_e03c3bd6`, `t_26f6bd79`, `t_19c3d874`, `t_892840da`, `t_e5a90026`, `t_6e863337` — Research Test Queries
- **What:** Test queries from dashboard "ρώτα τον Solomon" search box
- **Does:** Various test searches ("test", "test research query", "test search")
- **Practical value:** Validated Research tab search functionality
- **Use:** Smoke tests for Solomon search

---

## 📌 HOW TO USE THIS DOC

| Need | Action |
|------|--------|
| **Find what was built for X** | Search (Cmd+F) by keyword: "Orders", "Calendar", "Voice", "Wellness", "Money", etc. |
| **Know how to use a feature** | Check **Use** field — gives exact UI path or command |
| **Understand practical value** | Check **Practical value** — why it matters for daily workflow |
| **Trace implementation** | Task IDs (`t_xxxxxxxx`) link to Kanban for full context |
| **Onboard new agent** | Give this doc — complete system map in one file |

---

## 🔗 Related Files

- **Kanban Board:** http://100.107.122.62:9120 → Kanban tab
- **Dashboard:** http://100.107.122.62:9120
- **Obsidian Vault:** `/root/Documents/ObsidianVault`
  - `IT-LOGS.md` — Infrastructure changes
  - `STRATEGY-LOG.md` — Strategic decisions
  - `RESEARCH-LOG.md` — Solomon research output
  - `Wellness.md` — Fitness/food logs
  - `TODO.md` — Task parking lot
  - `Weekly Summaries/` — Generated weekly notes
- **Team Workflow Skill:** `hermes-agent-team-workflow` (in Hermes skills)