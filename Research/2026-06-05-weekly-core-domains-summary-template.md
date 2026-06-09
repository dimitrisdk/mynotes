# Weekly Summary Template for Core Domains

Task: `t_07835851`
Status: reusable template / proposal only
Raw-note policy: existing notes stay as-is; summaries link to source notes and do not delete, merge, or rewrite originals.
Timezone: Europe/Athens unless stated otherwise.

## Purpose

Create one weekly note that makes Dimitris's vault easy to scan across eight domains:
People, Finance, Health, Recipes, Research, Projects, Design, and Agent Logs.

The summary is not a replacement for source notes. It is a digest layer that:
- points to what changed;
- captures decisions and open loops;
- turns scattered notes into next actions;
- preserves gaps and uncertainty instead of hiding them.

## Recommended file location and naming

Preferred path:
`Weekly Summaries/YYYY-Www.md`

Fallback if a new folder is not desired yet:
`Agent Logs/Weekly/YYYY-Www-core-domains.md`

Example:
`Weekly Summaries/2026-W23.md`

## Frontmatter

```yaml
---
title: Weekly Core Domains Summary {{week}}
week: {{iso_week}}
period_start: {{YYYY-MM-DD}}
period_end: {{YYYY-MM-DD}}
timezone: Europe/Athens
status: draft # draft | reviewed | final
created: {{created_utc}}
created_by: {{agent_or_person}}
raw_notes_retained: true
source_policy: link-only-no-deletion
summary_domains:
  - people
  - finance
  - health
  - recipes
  - research
  - projects
  - design
  - agent-logs
tags:
  - weekly-summary
  - vault-index
---
```

## One-page weekly outline

# Weekly Core Domains Summary {{week}}

## 1. Executive scan
Use this section for 5–8 bullets maximum.

Fields:
- `Wins`: completed outcomes worth remembering.
- `Risks`: deadlines, health/finance issues, blocked projects, stale automations.
- `Decisions needed`: choices Dimitris should make.
- `Next 7 days`: the most important actions.

Template:
- Wins: {{top_win_1}}; {{top_win_2}}
- Risks / watchouts: {{risk_or_none}}
- Decisions needed: {{decision_1_or_none}}
- Next 7 days: {{action_1}}, {{action_2}}, {{action_3}}

## 2. Domain summaries
Each domain should stay short: 3–7 bullets plus an action list. Every non-obvious claim should link to source notes.

### People
What gets summarized:
- new or updated contacts, providers, family/friends context;
- commitments made to people;
- follow-ups, appointments, promised replies;
- relationship/context notes that affect projects, health, finance, or logistics.

Required sections:
- Notable interactions
- Commitments / follow-ups
- People to contact next week
- Source notes

Action rule:
Every person item should end with either `done`, `waiting`, `follow up by DATE`, or `no action`.

Template:
- Notable interactions:
  - {{person}} — {{context}} — source: [[...]]
- Commitments / follow-ups:
  - [ ] {{action}} — person: {{person}} — due: {{date_or_none}}
- Source notes: [[...]], [[...]]

### Finance
What gets summarized:
- weekly expense/investment movement;
- unusual spending;
- pending reimbursements, bills, subscriptions;
- finance ideas or product/market notes;
- data gaps in trackers.

Required sections:
- Cashflow / expenses
- Investments / assets
- Bills / subscriptions / reimbursements
- Finance ideas
- Data quality gaps

Action rule:
Use numbers only when source data exists. If totals are not verified, write `not calculated` rather than guessing.

Template:
- Cashflow / expenses: {{total_or_not_calculated}} — source: [[Finance/Monthly Finance Tracker]]
- Unusual items: {{item_or_none}}
- Investments / assets: {{update_or_none}}
- Bills / reimbursements:
  - [ ] {{finance_action}} — due: {{date}}
- Data gaps: {{missing_receipts_or_uncategorized_rows}}

### Health
What gets summarized:
- workouts/training, sleep/weight/wellness patterns if logged;
- medical appointments/notes;
- supplements/orders;
- symptoms or issues requiring follow-up;
- consistency trends, not over-analysis.

Required sections:
- Training / movement
- Wellness signals
- Medical / appointments
- Supplements / nutrition support
- Follow-ups

Action rule:
Do not infer medical advice. Separate `observed/logged` from `interpretation` and flag when data is thin.

Template:
- Training / movement: {{sessions_or_not_logged}} — source: [[Wellness]], [[Gymnastics]]
- Wellness signals: {{logged_pattern_or_insufficient_data}}
- Medical / appointments: {{medical_update_or_none}}
- Follow-ups:
  - [ ] {{health_action}} — due: {{date_or_none}}

### Recipes
What gets summarized:
- recipes added or changed;
- meal-prep plans;
- shopping-list items that recur;
- food experiments/results;
- hidden food spots or food ideas from Keep imports.

Required sections:
- New/updated recipes
- Meal prep candidates
- Shopping / pantry notes
- Tried this week / verdict
- Next cooking actions

Action rule:
Keep reusable recipes here; daily food logs stay linked from Health/Wellness.

Template:
- New/updated recipes: [[{{recipe_note}}]] — {{short_description}}
- Tried this week: {{dish}} — verdict: {{keep/change/drop}}
- Meal prep candidates:
  - [ ] {{meal_prep_action}}
- Shopping / pantry: {{recurring_item_or_none}}

### Research
What gets summarized:
- Solomon research reports created this week;
- key conclusions, not full reports;
- watchlist changes;
- decisions supported by research;
- open questions and source gaps.

Required sections:
- Completed research
- Important conclusions
- Watchlists / monitoring
- Open questions
- Decisions enabled

Action rule:
Every research item should include status and confidence when available.

Template:
- Completed research:
  - [[Research/{{note}}]] — status: {{status}} — confidence: {{confidence}} — conclusion: {{one_line}}
- Watchlists / monitoring: {{watchlist_update_or_none}}
- Open questions:
  - [ ] {{research_gap}} — needed for: {{decision}}
- Decisions enabled: {{decision_or_none}}

### Projects
What gets summarized:
- active project progress;
- specs created, implementation changes, decisions;
- blocked tasks and dependencies;
- kanban handoffs that matter to Dimitris;
- next milestones.

Required sections:
- Active projects
- Completed this week
- Blocked / waiting
- Decisions / scope changes
- Next milestones

Action rule:
Separate `project status` from `task noise`. Link kanban IDs or project notes when available.

Template:
- [[{{project}}]] — status: {{green_yellow_red}} — {{one_line_progress}}
- Completed:
  - {{deliverable}} — source: [[...]] / kanban `{{task_id}}`
- Blocked / waiting:
  - [ ] {{blocker}} — owner: {{owner}} — needed by: {{date_or_none}}
- Next milestones: {{milestone_1}}, {{milestone_2}}

### Design
What gets summarized:
- UI/UX ideas, references, screenshots/assets;
- dashboard/product design decisions;
- visual direction changes;
- unresolved design questions.

Required sections:
- New references / assets
- UI decisions
- Design questions
- Next design actions

Action rule:
Design summaries should point to concrete screens, assets, or decisions; avoid vague taste notes.

Template:
- New references / assets: [[{{design_note_or_asset}}]] — {{why_it_matters}}
- UI decisions: {{decision_or_none}}
- Design questions:
  - [ ] {{question}} — for project: [[{{project}}]]
- Next actions: {{design_action_or_none}}

### Agent Logs
What gets summarized:
- material work performed by Panagia, Petros, Samson, Ioudas, Solomon;
- automation/cron changes;
- user preferences captured;
- tool failures or recurring operational issues;
- links to raw logs and existing weekly agent summary.

Required sections:
- Agent activity by person
- Automations / integrations
- Preferences captured
- Operational issues
- Raw log coverage

Action rule:
Raw logs remain unchanged. Summaries can quote/link them but must not replace them.

Template:
- Panagia: {{material_outcome}} — source: [[Agent Logs/Panagia]]
- Solomon: {{material_outcome}} — source: [[RESEARCH-LOG]] / [[Agent Logs/Solomon]]
- Petros/Ioudas/Samson: {{material_outcome_or_none}}
- Automations / integrations: {{cron_or_gateway_update_or_none}}
- Operational issues:
  - [ ] {{issue}} — owner: {{owner_or_unknown}}
- Raw log coverage: {{files_checked}}; gaps: {{gaps_or_none}}

## 3. Cross-domain decisions
Use this when a decision touches multiple domains.

Fields:
- `Decision`: the choice to make or already made.
- `Domains affected`: one or more of People, Finance, Health, Recipes, Research, Projects, Design, Agent Logs.
- `Evidence`: source notes or facts.
- `Owner`: Dimitris or an agent/person.
- `Deadline`: date or `none`.

Template:
| Decision | Domains affected | Evidence | Owner | Deadline | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| {{decision}} | {{domains}} | [[...]] | {{owner}} | {{date_or_none}} | proposed/decided/waiting |

## 4. Open loops and next actions
This is the most important operational section. Keep it short and sortable.

Fields:
- `Action`: concrete verb-led next step.
- `Domain`: primary domain.
- `Owner`: Dimitris / agent / external person.
- `Due`: date or `none`.
- `Source`: source note or task.
- `Priority`: P1, P2, P3.

Template:
| Priority | Action | Domain | Owner | Due | Source |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | {{verb_led_action}} | {{domain}} | {{owner}} | {{date}} | [[...]] |

Priority definitions:
- P1: affects money, health, deadline, or blocked project.
- P2: useful progress this week.
- P3: nice-to-have / backlog.

## 5. Source coverage and gaps
Required so the summary is auditable.

Fields:
- `Sources checked`: notes/folders reviewed.
- `Sources not checked`: known relevant sources skipped.
- `Gaps`: missing data, stale notes, unclear ownership.
- `Confidence`: high / medium / low.

Template:
- Sources checked:
  - [[...]] — {{coverage_note}}
- Sources not checked:
  - {{source_or_none}}
- Gaps:
  - {{gap_or_none}}
- Confidence: {{high_medium_low}} — {{reason}}

## 6. Raw-note retention statement

Required text:
`Raw/source notes were retained unchanged. This weekly summary links to existing notes and does not assume deletion, consolidation, moving, or rewriting of source material.`

## Field definitions

- `week`: ISO week label, e.g. `2026-W23`.
- `period_start` / `period_end`: covered dates in Europe/Athens.
- `status`: `draft`, `reviewed`, or `final`.
- `source_policy`: must remain `link-only-no-deletion` unless Dimitris explicitly approves migration/cleanup.
- `raw_notes_retained`: true if source notes were not modified/deleted as part of summarization.
- `green_yellow_red`: green = on track, yellow = needs attention, red = blocked/urgent.
- `confidence`: high = source notes are complete and current; medium = enough signal but some gaps; low = sparse or stale sources.
- `owner`: the person/agent responsible for the next action, not merely mentioned in the note.
- `due`: explicit date; use `none` if no deadline is known.
- `source`: Obsidian wikilink, kanban task ID, or file path that supports the summary item.

## Scanability rules

1. Start with the Executive scan; no more than 8 bullets.
2. Use checkboxes only for actions, not for facts.
3. Link sources inline; do not paste long raw excerpts unless necessary.
4. Keep each domain to one screen where possible.
5. Use tables only for decisions/actions; bullets for narrative updates.
6. Mark missing data explicitly: `not logged`, `not checked`, `not calculated`, or `unknown`.
7. Prefer verbs in actions: `send`, `book`, `review`, `decide`, `pay`, `verify`, `ask`, `ship`.
8. Do not mix facts and interpretation. If interpretation is needed, label it.

## Brief filled example

```markdown
---
title: Weekly Core Domains Summary 2026-W23
week: 2026-W23
period_start: 2026-06-01
period_end: 2026-06-07
timezone: Europe/Athens
status: draft
created: 2026-06-05 07:43 UTC
created_by: Solomon
raw_notes_retained: true
source_policy: link-only-no-deletion
summary_domains: [people, finance, health, recipes, research, projects, design, agent-logs]
tags: [weekly-summary, vault-index]
---

# Weekly Core Domains Summary 2026-W23

## 1. Executive scan
- Wins: Obsidian taxonomy proposal completed; Research tab contract defined.
- Risks / watchouts: Finance totals not recalculated in this summary; treat as unverified.
- Decisions needed: choose whether weekly summaries live in `Weekly Summaries/` or `Agent Logs/Weekly/`.
- Next 7 days: review dashboard Researches implementation, verify finance tracker data, pick active project priorities.

## 2. Domain summaries

### People
- Notable interactions: not checked in this pass.
- Commitments / follow-ups:
  - [ ] Review People notes once folder/index exists — person: Dimitris — due: none
- Source notes: none checked

### Finance
- Cashflow / expenses: not calculated — source: [[Finance/Monthly Finance Tracker]]
- Unusual items: unknown; tracker not audited for this example.
- Bills / reimbursements: none identified.
- Data gaps: weekly totals need explicit calculation before final summary.

### Health
- Training / movement: not checked — possible sources: [[Wellness]], [[Gymnastics]]
- Medical / appointments: none summarized.
- Follow-ups:
  - [ ] Check whether any medical Keep imports need weekly follow-up — due: none

### Recipes
- New/updated recipes: not checked — possible source: [[Recipes]] and Keep Imports.
- Tried this week: not logged.
- Meal prep candidates: none selected.

### Research
- Completed research:
  - [[Research/2026-06-05-obsidian-vault-taxonomy-proposal]] — status: completed — confidence: medium-high — conclusion: preserve existing notes, add domain summaries/linking layer.
- Open questions:
  - [ ] Decide weekly summary note location — needed for automation.

### Projects
- [[Dashboard Refresh Plan]] — status: yellow — multiple specs exist; implementation sequence needs review.
- Completed:
  - Research tab data/interaction spec — kanban `t_7baa6b3e`
  - Scheduled jobs UI contract — kanban `t_6004b28d`
- Blocked / waiting:
  - [ ] Confirm start gates before implementation — owner: Dimitris — needed by: none

### Design
- New references / assets: not checked.
- UI decisions: Researches tab should use markdown-backed entries from `RESEARCH-LOG.md`.
- Design questions:
  - [ ] How compact should the weekly summary dashboard card be? — for project: [[Dashboard Refresh Plan]]

### Agent Logs
- Solomon: completed several specs/research notes — source: [[RESEARCH-LOG]]
- Automations / integrations: scheduled jobs UI contract defined; no cron changes made here.
- Raw log coverage: `RESEARCH-LOG.md` checked; other raw logs not checked.

## 3. Cross-domain decisions
| Decision | Domains affected | Evidence | Owner | Deadline | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Weekly summaries should be link-only and preserve raw notes | all | [[Research/2026-06-05-obsidian-vault-taxonomy-proposal]] | Dimitris | none | proposed |

## 4. Open loops and next actions
| Priority | Action | Domain | Owner | Due | Source |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | Decide canonical location for weekly summaries | Projects | Dimitris | none | this template |
| P2 | Generate first real weekly summary from source notes | Agent Logs | Solomon/Panagia | none | this template |
| P2 | Verify finance totals before including them | Finance | Samson/Petros | none | [[Finance/Monthly Finance Tracker]] |

## 5. Source coverage and gaps
- Sources checked: [[RESEARCH-LOG]], [[Research/2026-06-05-obsidian-vault-taxonomy-proposal]], [[Agent Logs/Weekly/_template-agent-weekly-summary]]
- Sources not checked: People, Finance tracker rows, Wellness, Recipes, Design assets.
- Gaps: this is a format example, not a real weekly audit.
- Confidence: medium — template aligns with current vault taxonomy, but first live run may reveal adjustments.

## 6. Raw-note retention statement
Raw/source notes were retained unchanged. This weekly summary links to existing notes and does not assume deletion, consolidation, moving, or rewriting of source material.
```
