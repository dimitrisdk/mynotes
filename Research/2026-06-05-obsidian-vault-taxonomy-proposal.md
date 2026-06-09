# Obsidian Vault Taxonomy Proposal

Date: 2026-06-05 02:57 UTC
Task: `t_83991243`
Status: proposal only — no deletion, merge, or rename performed
Confidence: medium-high

## Συμπέρασμα

- The vault is already usable but uneven: Research, Finance, Agent Logs, and Keep Imports have recognizable homes; People, Projects, Design, Health, and Recipes are still partly root-level or mixed into Keep Imports/TODO.
- Keep Imports must remain protected. The approved non-archived categories are: recipes/meal prep, poems, finance/ideas, medical notes, guitar notes, and hidden food spots.
- Recommended direction: keep logs and raw/imported material stable, add canonical top-level domains for People, Finance, Health, Recipes, Research, Projects, Design, and Agent Logs, then migrate by linking first, moving later only after Dimitris approval.

## Audit facts

Sources reviewed:
- `RESEARCH-LOG.md`
- `TODO.md`
- `Dashboard Refresh Plan.md`
- `Keep Import Cleanup Review.md`
- `Agent Logs/obsidian-cleanup-2026-06-04.md`
- `Agent Logs/Panagia.md`
- `Finance/Monthly Finance Tracker.md`
- `Wellness.md`
- `Wellness Data Contract.md`
- `Recipes.md`
- representative files under `Keep Imports/`

Current visible structure:
- Root notes: operational/log/capture notes such as `TODO.md`, `AI Inbox.md`, `Dashboard Refresh Plan.md`, `Wellness.md`, `Gymnastics.md`, `Recipes.md`, `MCP-Research.md`, `Poems.md`, `IT-LOGS.md`, `STRATEGY-LOG.md`, `RESEARCH-LOG.md`.
- `Research/`: dated research reports using `YYYY-MM-DD-slug.md` naming.
- `Finance/`: `Monthly Finance Tracker.md`, monthly expense notes, and `finance-tracker.csv`.
- `Agent Logs/`: per-agent/event logs plus `News Briefs/` and `Weekly/` template.
- `Briefs/`: morning/news briefs, currently separate from `Agent Logs/News Briefs/`.
- `Keep Imports/`: remaining non-archived imported Keep notes plus `Assets/` images.

Existing tag/naming patterns:
- Keep imports mainly use first-line tags like `#Keep/Colour/DEFAULT`; cleanup logs reference removed `#Keep/Archived` notes.
- Research notes use dated slugs and metadata lines: Date, Status, Confidence, Kanban/Artifact when relevant.
- Wellness generated blocks use stable HTML markers such as `<!-- wellness:...:start -->` / `end`.
- Finance tracker uses table rows and a category taxonomy, not Obsidian tags.
- A consistent global tag taxonomy is not yet established.

Duplicate / overlapping concepts:
- Wellness / Health / Training / Recipes: `Wellness.md`, `Wellness Data Contract.md`, `Gymnastics.md`, `Recipes.md`, and multiple meal-prep/medical Keep imports overlap.
- Briefs / Agent logs: `Briefs/` and `Agent Logs/News Briefs/` both contain brief-like material.
- Projects / Research / Strategy: `Dashboard Refresh Plan.md`, `MCP-Research.md`, `STRATEGY-LOG.md`, research specs, and Keep project/business idea notes overlap.
- Capture / Tasks / Keep: `AI Inbox.md`, `TODO.md`, and `Keep Imports/` are all used as intake/backlog surfaces.
- Finance / Ideas: canonical finance tracking exists in `Finance/`, but finance/product ideas also exist in Keep Imports.

## Proposed top-level vault structure

```text
/People/
/Finance/
/Health/
/Recipes/
/Research/
/Projects/
/Design/
/Agent Logs/
/Briefs/                 optional: keep for user-facing briefs, or link into Agent Logs
/Keep Imports/           preserve as raw/imported archive until explicit migration approval
/Inbox/                  optional future canonical capture area
```

### People

Purpose: contacts, family/friends, providers, relationship/context notes.

Suggested folders:
- `People/Contacts/`
- `People/Providers/` — doctors, dentists, gyms, accountants, vendors, if Dimitris wants them as people/org records.
- `People/Family & Friends/`

Rules:
- Do not store raw PII beyond what is already intentionally captured.
- Medical providers can be linked from both `People/Providers/` and `Health/Medical/`.
- Names appearing in TODO/Keep should be linked first; move only after review.

### Finance

Purpose: expenses, investments, assets, finance documents, finance/product ideas.

Suggested folders:
- `Finance/Monthly Expenses/` — current monthly notes.
- `Finance/Investments/` — ETF contributions, broker notes, asset labels.
- `Finance/Reports/` — monthly/quarterly summaries.
- `Finance/Data/` — CSV/exports if kept in vault.
- `Finance/Ideas/` — finance app/product ideas from Keep, including AI finance advisor concepts.

Rules:
- Preserve existing `Finance/Monthly Finance Tracker.md` schema.
- Keep Amount positive and Type as Expense/Investment unless future redesign changes it.
- Avoid storing bank/card/account identifiers.

### Health

Purpose: wellness ledger, training, weight, medical notes, supplement/order notes.

Suggested folders:
- `Health/Wellness.md` — eventual location for current `Wellness.md` projection.
- `Health/Data Contracts/` — `Wellness Data Contract.md`.
- `Health/Training/` — `Gymnastics.md`, BJJ/Yoga/gym programs.
- `Health/Medical/` — doctor/dentist notes and appointments.
- `Health/Supplements/` — protein/supplement notes and orders.

Rules:
- Dashboard remains canonical for structured wellness records; Obsidian is projection/prose.
- Keep generated `wellness:` markers untouched.
- Medical notes from Keep are approved to preserve.

### Recipes

Purpose: recipes, meal prep, shopping lists specifically tied to food planning.

Suggested folders:
- `Recipes/Index.md`
- `Recipes/Ninja Creami/`
- `Recipes/Meal Prep/`
- `Recipes/Shopping Lists/`

Rules:
- Current `Recipes.md` can become an index or collection note later.
- Approved Keep categories `recipes/meal prep` and hidden food spots must be preserved.
- Food logs stay in Health/Wellness; reusable recipes stay in Recipes.

### Research

Purpose: Solomon research reports and sourced decision briefs.

Suggested folders:
- `Research/YYYY-MM-DD-slug.md` — keep existing convention.
- `Research/Sources/` — optional future raw extracts/source notes.
- `Research/Watchlists/` — optional recurring areas: AI, Hermes, UX, ETFs, BJJ.

Rules:
- `RESEARCH-LOG.md` remains the index for dashboard Researches.
- Research notes should include conclusion, facts/sources, assumptions/gaps, interpretation, and implication for Dimitris.
- Keep source URLs and local artifact paths when relevant.

### Projects

Purpose: active initiatives and build/product work.

Suggested folders:
- `Projects/Hermes Dashboard/`
- `Projects/Agent OS/`
- `Projects/Home Assistant/`
- `Projects/Product Ideas/`
- `Projects/Archive/`

Rules:
- Use one project index note per project: `Overview.md`, `Roadmap.md`, `Decisions.md`, `Tasks.md`, `Artifacts.md` as needed.
- Kanban remains source of truth for task execution; Obsidian stores specs, decisions, summaries, and links.
- Keep business/project ideas from Keep Imports preserved; migrate only after approval.

### Design

Purpose: UI/UX references, design ideas, visual assets, brand directions.

Suggested folders:
- `Design/UI Ideas/`
- `Design/References/`
- `Design/Assets/`
- `Design/Branding/`

Rules:
- Design-related Keep concepts should be tagged/linked first, not moved immediately.
- Dashboard visual specs from `Dashboard Refresh Plan.md` can link to Design notes once project folders exist.

### Agent Logs

Purpose: durable activity logs and summaries for Panagia, Petros, Samson, Ioudas, Solomon, plus system/news/weekly logs.

Suggested folders:
- `Agent Logs/Panagia.md`, `Petros.md`, `Samson.md`, `Ioudas.md`, `Solomon.md`.
- `Agent Logs/News Briefs/YYYY-MM-DD.md`.
- `Agent Logs/Weekly/YYYY-MM-DD.md` plus existing weekly template.
- `Agent Logs/System/` for cleanup/deploy/sync operations.

Rules:
- Do not rewrite raw historical logs unless explicitly asked.
- Weekly summaries may quote/link raw logs but should not replace them.
- Decide later whether root `Briefs/` remains user-facing or moves under `Agent Logs/Briefs/`.

## Folder/tag conventions

Folder conventions:
- Use English folder names for stable automation: `Research/`, `Finance/`, `Health/`, etc.
- Use readable note titles; for dated reports use `YYYY-MM-DD-slug.md`.
- Keep raw imports under `Keep Imports/` until approved migration.
- Prefer index notes for major folders: `Finance/Index.md`, `Health/Index.md`, `Projects/Index.md`.

Tag conventions:
- Reserve `#Keep/...` only for imported Keep metadata.
- Suggested domain tags: `#people`, `#finance`, `#health`, `#recipes`, `#research`, `#project`, `#design`, `#agent-log`.
- Suggested status tags: `#status/draft`, `#status/active`, `#status/archived`, `#status/needs-review`.
- Suggested source tags: `#source/keep`, `#source/telegram`, `#source/dashboard`, `#source/kanban`, `#source/web`.
- Do not over-tag notes that already live in clear folders; use tags for cross-cutting state/source.

## Migration notes — no action taken

Safe first-pass migration sequence:
1. Create indexes only; do not move content.
2. Add links from indexes to existing notes.
3. Tag or annotate Keep Imports with approved category labels if needed.
4. Review with Dimitris.
5. Only after approval, move notes one domain at a time.
6. Keep a migration log under `Agent Logs/System/`.

Suggested mapping:
- `Wellness.md` -> `Health/Wellness.md` after sync tooling paths are updated.
- `Wellness Data Contract.md` -> `Health/Data Contracts/Wellness Data Contract.md`.
- `Gymnastics.md` -> `Health/Training/Gymnastics.md`.
- `Recipes.md` -> `Recipes/Index.md` or `Recipes/Recipe Collection.md`.
- `Dashboard Refresh Plan.md` -> `Projects/Hermes Dashboard/Dashboard Refresh Plan.md`.
- `MCP-Research.md` -> either `Research/` if research report, or `Projects/Agent OS/` if project spec.
- `Poems.md` remains outside requested domains unless a future `Creative/` or `Writing/` area is approved.
- `Briefs/` remains unchanged until a decision is made about user-facing briefs vs agent logs.

## Keep Import categories that must be preserved

Explicitly approved categories from `Keep Import Cleanup Review.md` and `Agent Logs/Panagia.md`:

1. Recipes / meal prep
2. Poems
3. Finance / ideas
4. Medical notes
5. Guitar notes
6. Hidden food spots

Operational preservation rule:
- Do not delete these categories.
- Do not merge them into canonical folders without review.
- Keep original imported files and assets available until the migration has been verified.
- If moved later, preserve source filename, Keep metadata line, labels/tags, timestamps if available, checklist state if any, attachments/assets, and backlinks to the canonical note.

## Assumptions / gaps

- This audit used visible markdown notes and representative Keep files; it did not parse every attachment image in `Keep Imports/Assets/`.
- Some categories are inferred from filenames/content; exact Keep labels beyond `#Keep/Colour/DEFAULT` are not visible in the current markdown files.
- People and Design domains are underdeveloped in the current vault; proposal is based on requested scope plus scattered existing notes.

## Εκτίμηση

The main risk is moving too early. The current vault is small enough that a link-first/index-first taxonomy will give Dimitris better navigation without breaking dashboard/agent sync paths. Keep Imports should be treated as protected raw material, not clutter, because Dimitris already approved specific categories to preserve.

## Τι σημαίνει για τον Δημήτρη

Dimitris can approve a clean taxonomy without losing old Keep knowledge: first add indexes and links, then migrate only reviewed categories. The highest-value immediate wins are `Health/`, `Recipes/`, `Projects/Hermes Dashboard/`, and a protected Keep Imports index.
