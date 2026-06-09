# Obsidian domain folder audit

Date: 2026-06-05 07:43 UTC
Task: `t_033bb9ab`
Status: audit/recommendations only — no deletions, merges, or renames performed
Confidence: high for visible markdown/folder structure; medium for inferred Keep Import categories

## Συμπέρασμα

- The requested domains are only partially represented as top-level folders. Existing top-level folders: `Finance/`, `Research/`, `Agent Logs/`. Missing top-level folders: `People/`, `Health/`, `Recipes/`, `Projects/`, `Design/`.
- Several missing domains already have content, but it is root-level or still inside `Keep Imports/`: Health maps mainly to `Wellness.md`, `Wellness Data Contract.md`, `Gymnastics.md`, and medical/training Keep notes; Recipes maps to `Recipes.md` and meal-prep Keep notes; Projects/Design map mostly to `Dashboard Refresh Plan.md`, `TODO.md`, `MCP-Research.md`, and product/design Keep notes.
- Do not migrate yet. The safer next step is to create index notes/folders first, link existing notes from them, then move files only after Dimitris approves each domain.

## Current inventory by requested domain

### People

Current state:
- No `People/` top-level folder exists.
- People/provider/contact-like material appears scattered in root/Keep imports and old cleanup logs, not in a canonical people area.
- Relevant examples from remaining `Keep Imports/`: `Lynda.md`, `Odotiatros.md`, plus possible names/places in `Trakarisma.md`, `Xares bobou.md`, `New york.md`, `Souvkaki.md`.
- Cleanup log shows many old person/provider/location notes were deleted only when archived, e.g. `David.md`, `Diana.md`, `Logistria.md`, `Orthopedic.md`, `Physio.md`, `Kourema...`, `Vagus vasilis.md`, etc.

Unclear/inconsistent:
- The vault has no rule separating people, providers, places, and errands.
- Some provider notes may belong both to `People/Providers/` and `Health/Medical/`.

Recommended target:
- `People/Index.md`
- `People/Contacts/`
- `People/Providers/`
- `People/Family & Friends/`
- Optional later: `People/Places/` only if Dimitris wants restaurants/venues stored as people/place records; otherwise put food spots under `Recipes/Food Spots/`.

Risks/ambiguities:
- PII/contact data should not be normalized or expanded without approval.
- Some names are ambiguous and may be personal, provider, or business references.

### Finance

Current state:
- `Finance/` exists with 2 markdown files and 2 subfolders.
- Immediate contents:
  - `Finance/Monthly Finance Tracker.md`
  - `Finance/finance-tracker.csv`
  - `Finance/Monthly Expenses/2026-06.md`
  - `Finance/Calendar/` empty
- Finance/product idea content also exists in `Keep Imports/`:
  - `Project Brief_ AI Personal Finance Advisor.md`
  - `Οικονομία.md`
  - `K........ A,,  +,,  +,,,,,,.md` preview references Eurobank
  - business/app idea notes such as `Business ideas.md`, `busineess idea.md`, `Saas.md`, `Meetup.md`, `idea.md`, `iodea.md`, etc.

Unclear/inconsistent:
- Operational finance tracking is canonical under `Finance/`, but finance/product ideas are mixed with general project ideas in `Keep Imports/`.
- `Finance/Calendar/` exists but appears empty; purpose is unclear.

Recommended target:
- Keep current tracker paths stable until finance tooling is checked.
- Add:
  - `Finance/Index.md`
  - `Finance/Monthly Expenses/`
  - `Finance/Data/` for CSV/export files if Dimitris wants data files inside vault
  - `Finance/Reports/`
  - `Finance/Investments/`
  - `Finance/Ideas/` for finance product concepts
- Clarify whether `Finance/Calendar/` is needed; if not, mark as candidate for later review, not deletion.

Risks/ambiguities:
- Do not store bank/card/account identifiers.
- Moving `finance-tracker.csv` could break automations or formulas unless paths are updated.

### Health

Current state:
- No `Health/` top-level folder exists.
- Health content lives mostly at root:
  - `Wellness.md` — human-readable wellness ledger/projection from dashboard
  - `Wellness Data Contract.md` — sync/data-contract note
  - `Gymnastics.md` — training/program content
- Relevant remaining `Keep Imports/`:
  - `Πρόγραμμα Γυμναστηρίου (BJJ-Yoga).md`
  - `Gym.md`
  - `Αντιφλεγμ.md`
  - `Odotiatros.md`
  - `Ακτίνα.md` / other possible medical/provider fragments
- `Recipes.md` and meal prep notes overlap with health, but reusable recipes should not be mixed with wellness logs.

Unclear/inconsistent:
- `Wellness.md` is both a user-facing note and generated dashboard projection; moving it has automation risk.
- Training, medical, supplements, and food/recipes are currently mixed conceptually.

Recommended target:
- `Health/Index.md`
- `Health/Wellness.md` or keep root `Wellness.md` until sync paths are updated
- `Health/Data Contracts/Wellness Data Contract.md`
- `Health/Training/`
- `Health/Medical/`
- `Health/Supplements/`

Risks/ambiguities:
- Do not edit generated blocks in `Wellness.md` marked with `<!-- wellness:... -->`.
- Moving wellness notes can break dashboard/Samson sync unless code/config paths are audited first.
- Medical/provider notes may contain sensitive personal health data.

### Recipes

Current state:
- No `Recipes/` top-level folder exists.
- Root `Recipes.md` exists and contains:
  - sugar-free granola recipes
  - Ninja Creami recipe collection
- Relevant remaining `Keep Imports/`:
  - `Η συνταγή μου για Porridge.md`
  - `Λίστα Supermarket (Meal Prep).md`
  - `Ψώνια από Supermarket - Μανάβικο.md`
  - `Ψώνια από Κρεοπωλείο - Φρέσκα.md`
  - `Ψώνια για Meal Prep (Ελληνικό Πλάνο).md`
  - `Panakia.md`
  - `Souvkaki.md`
  - possibly `iodea.md` as a recipe-app idea, not an actual recipe

Unclear/inconsistent:
- Shopping lists, meal prep, reusable recipes, hidden food spots, and health food logs are not separated.
- `Recipes.md` is a collection note, not a folder/index.

Recommended target:
- `Recipes/Index.md`
- `Recipes/Ninja Creami/`
- `Recipes/Breakfast & Snacks/`
- `Recipes/Meal Prep/`
- `Recipes/Shopping Lists/`
- `Recipes/Food Spots/` for hidden food spots/restaurants if Dimitris wants them preserved here

Risks/ambiguities:
- Food logs and macro records should stay in Health/Wellness/dashboard, not in Recipes.
- Keep original Keep files until migration review because approved remaining Keep categories include recipes/meal prep and hidden food spots.

### Research

Current state:
- `Research/` exists with 10 dated markdown reports.
- Current files:
  - `2026-06-03-firecrawl-workflow-smoke-test.md`
  - `2026-06-03-side-hustles-dimitris.md`
  - `2026-06-03-top-github-repos-build-integration.md`
  - `2026-06-04-agent-logs-weekly-summary-workflow.md`
  - `2026-06-04-home-server-local-llm-greece.md`
  - `2026-06-04-unified-search-ask-my-os-mvp-scope.md`
  - `2026-06-05-hermes-audio-video-stt-tts-telegram.md`
  - `2026-06-05-obsidian-vault-taxonomy-proposal.md`
  - `2026-06-05-scheduled-jobs-ui-contract.md`
  - `2026-06-05-unified-search-retrieval-answer-pipeline-design.md`
- Root research/log indexes and related notes:
  - `RESEARCH-LOG.md`
  - `MCP-Research.md`
  - `Dashboard Refresh Plan.md` includes Researches dashboard requirements

Unclear/inconsistent:
- `MCP-Research.md` is root-level although its name suggests Research or Projects.
- Some Research notes are implementation specs, which may later belong under `Projects/.../Specs/` with links from Research.

Recommended target:
- Preserve current `Research/YYYY-MM-DD-slug.md` convention.
- Add optional subfolders only if volume grows:
  - `Research/Sources/`
  - `Research/Watchlists/`
  - `Research/Briefs/`
- Keep `RESEARCH-LOG.md` as dashboard-visible index.

Risks/ambiguities:
- Over-foldering Research now would add friction; flat dated reports are working.
- Moving research specs may break dashboard Researches tab if it indexes `RESEARCH-LOG.md` + `Research/`.

### Projects

Current state:
- No `Projects/` top-level folder exists.
- Project/spec content is scattered:
  - `Dashboard Refresh Plan.md`
  - `TODO.md`
  - `MCP-Research.md`
  - `AI Inbox.md`
  - `STRATEGY-LOG.md`
  - several `Research/2026-06-04/05-*` implementation specs
  - `Keep Imports/Project Brief_ AI Personal Finance Advisor.md`
  - `Keep Imports/Saas.md`, `Business ideas.md`, `busineess idea.md`, `idea.md`, `Ideas.md`, `iodea.md`, `ieda.md`, `Meetup.md`

Unclear/inconsistent:
- Active projects, product ideas, research specs, and task backlog are mixed across root, Research, and Keep Imports.
- Kanban is the execution source of truth; Obsidian should not duplicate live task state too aggressively.

Recommended target:
- `Projects/Index.md`
- `Projects/Hermes Dashboard/`
  - `Overview.md`, `Dashboard Refresh Plan.md`, `Researches Tab.md`, `Decisions.md`
- `Projects/Agent OS/`
- `Projects/Personal Finance Advisor/`
- `Projects/Product Ideas/`
- `Projects/Archive/`

Risks/ambiguities:
- Moving project specs out of Research could hide them from the Researches dashboard unless `RESEARCH-LOG.md` links are preserved.
- Product ideas from Keep Imports should be categorized/link-indexed before moving; some are rough fragments.

### Design

Current state:
- No `Design/` top-level folder exists.
- Design material is sparse and scattered:
  - `Dashboard Refresh Plan.md` contains UI/dashboard interaction decisions
  - `Keep Imports/idea.md` preview: UI designs generating Dribbble/Behance shots
  - old deleted archived notes in cleanup log included `Design bakery promotion.md`, `Dribbble ideaa.md`, `Ux blog.md`, `Ux workshop.md`, `Ux.md`, `Caricature photoshop.md`, `Sketch videos.md`, `sketch licence.md`
- There is no canonical design assets/reference structure.

Unclear/inconsistent:
- Design could mean dashboard UX specs, visual references, brand/creative notes, or product inspiration; current vault does not distinguish them.

Recommended target:
- `Design/Index.md`
- `Design/UI Ideas/`
- `Design/References/`
- `Design/Branding/`
- `Design/Assets/`
- Link design specs from `Projects/Hermes Dashboard/` rather than duplicating them.

Risks/ambiguities:
- Creating Design too early may create an empty folder; best first step is an index that links current design-related notes.
- Some design material existed only in archived Keep notes that were already deleted per prior cleanup, so the remaining design corpus is thin.

### Agent Logs

Current state:
- `Agent Logs/` exists with 5 markdown files and 2 subfolders.
- Immediate contents:
  - `Agent Logs/Panagia.md`
  - `Agent Logs/obsidian-cleanup-2026-06-04.md`
  - `Agent Logs/2026-06-04-news-brief.md`
  - `Agent Logs/News Briefs/2026-06-03.md`
  - `Agent Logs/Weekly/_template-agent-weekly-summary.md`
- Related root logs:
  - `IT-LOGS.md`
  - `STRATEGY-LOG.md`
  - `RESEARCH-LOG.md`
- Separate brief folder also exists:
  - `Briefs/` with 5 morning/news briefs
  - `News/Daily News Briefs/2026-06-05.md`

Unclear/inconsistent:
- `Briefs/`, `News/Daily News Briefs/`, and `Agent Logs/News Briefs/` overlap.
- Only `Panagia.md` exists as per-agent log; no `Solomon.md`, `Petros.md`, `Samson.md`, or `Ioudas.md` yet.
- System/cleanup logs are mixed directly under `Agent Logs/`.

Recommended target:
- Preserve `Agent Logs/` as canonical operational log home.
- Add:
  - `Agent Logs/System/`
  - `Agent Logs/Weekly/`
  - `Agent Logs/News Briefs/`
  - `Agent Logs/Agents/Panagia.md`, `Solomon.md`, `Petros.md`, `Samson.md`, `Ioudas.md` — or keep per-agent files directly under `Agent Logs/` if simplicity wins.
- Decide whether `Briefs/` remains user-facing output and `Agent Logs/` remains machine/operational history.

Risks/ambiguities:
- Moving logs can break jobs or human expectations if paths are hardcoded.
- News briefs have three homes; consolidate only after identifying which jobs write each path.

## Cross-domain recommendations

1. Use a link-first migration:
   - create only domain `Index.md` notes first;
   - link existing root/Keep/Research notes from the relevant index;
   - do not move files until Dimitris approves a domain-by-domain migration.
2. Keep `Keep Imports/` as protected raw/imported source until migration is reviewed. Prior approved categories to preserve: recipes/meal prep, poems, finance/ideas, medical notes, guitar notes, hidden food spots.
3. Treat automation-sensitive files as path-locked until code paths are checked:
   - `Wellness.md`
   - `Wellness Data Contract.md`
   - `Finance/Monthly Finance Tracker.md`
   - `Finance/finance-tracker.csv`
   - `RESEARCH-LOG.md`
   - `Agent Logs/*`
4. Prefer English canonical folder names for automation; keep Greek/natural note titles where they are user-authored content.
5. If folders are approved, create indexes before moving content:
   - `People/Index.md`
   - `Finance/Index.md`
   - `Health/Index.md`
   - `Recipes/Index.md`
   - `Research/Index.md` optional; `RESEARCH-LOG.md` already functions as index
   - `Projects/Index.md`
   - `Design/Index.md`
   - `Agent Logs/Index.md`

## Sources reviewed

- Vault filesystem inventory under `/root/Documents/ObsidianVault`
- `Research/2026-06-05-obsidian-vault-taxonomy-proposal.md`
- `Keep Import Cleanup Review.md`
- `Agent Logs/obsidian-cleanup-2026-06-04.md`
- `Finance/Monthly Finance Tracker.md`
- `Recipes.md`
- `Wellness.md`
- Remaining markdown files under `Keep Imports/`

## Τι σημαίνει για τον Δημήτρη

Dimitris can approve a clean folder taxonomy without risking data loss or breaking automations. The highest-value next move is not a bulk cleanup; it is to create domain indexes and link existing scattered notes, then approve real moves one domain at a time.
