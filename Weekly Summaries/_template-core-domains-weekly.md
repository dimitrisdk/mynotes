---
title: Weekly Core Domains Summary {{week}}
week: {{iso_week}}
period_start: {{YYYY-MM-DD}}
period_end: {{YYYY-MM-DD}}
timezone: Europe/Athens
status: draft
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
tags: [weekly-summary, vault-index]
---

# Weekly Core Domains Summary {{week}}

## 1. Executive scan
- Wins: {{top_win_1}}; {{top_win_2}}
- Risks / watchouts: {{risk_or_none}}
- Decisions needed: {{decision_1_or_none}}
- Next 7 days: {{action_1}}, {{action_2}}, {{action_3}}

## 2. Domain summaries

### People
- Notable interactions:
  - {{person}} — {{context}} — source: [[...]]
- Commitments / follow-ups:
  - [ ] {{action}} — person: {{person}} — due: {{date_or_none}}
- Source notes: [[...]], [[...]]

### Finance
- Cashflow / expenses: {{total_or_not_calculated}} — source: [[Finance/Monthly Finance Tracker]]
- Unusual items: {{item_or_none}}
- Investments / assets: {{update_or_none}}
- Bills / reimbursements:
  - [ ] {{finance_action}} — due: {{date}}
- Data gaps: {{missing_receipts_or_uncategorized_rows}}

### Health
- Training / movement: {{sessions_or_not_logged}} — source: [[Wellness]], [[Gymnastics]]
- Wellness signals: {{logged_pattern_or_insufficient_data}}
- Medical / appointments: {{medical_update_or_none}}
- Follow-ups:
  - [ ] {{health_action}} — due: {{date_or_none}}

### Recipes
- New/updated recipes: [[{{recipe_note}}]] — {{short_description}}
- Tried this week: {{dish}} — verdict: {{keep/change/drop}}
- Meal prep candidates:
  - [ ] {{meal_prep_action}}
- Shopping / pantry: {{recurring_item_or_none}}

### Research
- Completed research:
  - [[Research/{{note}}]] — status: {{status}} — confidence: {{confidence}} — conclusion: {{one_line}}
- Watchlists / monitoring: {{watchlist_update_or_none}}
- Open questions:
  - [ ] {{research_gap}} — needed for: {{decision}}
- Decisions enabled: {{decision_or_none}}

### Projects
- [[{{project}}]] — status: {{green_yellow_red}} — {{one_line_progress}}
- Completed:
  - {{deliverable}} — source: [[...]] / kanban `{{task_id}}`
- Blocked / waiting:
  - [ ] {{blocker}} — owner: {{owner}} — needed by: {{date_or_none}}
- Next milestones: {{milestone_1}}, {{milestone_2}}

### Design
- New references / assets: [[{{design_note_or_asset}}]] — {{why_it_matters}}
- UI decisions: {{decision_or_none}}
- Design questions:
  - [ ] {{question}} — for project: [[{{project}}]]
- Next actions: {{design_action_or_none}}

### Agent Logs
- Panagia: {{material_outcome}} — source: [[Agent Logs/Panagia]]
- Solomon: {{material_outcome}} — source: [[RESEARCH-LOG]] / [[Agent Logs/Solomon]]
- Petros/Ioudas/Samson: {{material_outcome_or_none}}
- Automations / integrations: {{cron_or_gateway_update_or_none}}
- Operational issues:
  - [ ] {{issue}} — owner: {{owner_or_unknown}}
- Raw log coverage: {{files_checked}}; gaps: {{gaps_or_none}}

## 3. Cross-domain decisions
| Decision | Domains affected | Evidence | Owner | Deadline | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| {{decision}} | {{domains}} | [[...]] | {{owner}} | {{date_or_none}} | proposed/decided/waiting |

## 4. Open loops and next actions
| Priority | Action | Domain | Owner | Due | Source |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | {{verb_led_action}} | {{domain}} | {{owner}} | {{date}} | [[...]] |

Priority definitions:
- P1: affects money, health, deadline, or blocked project
- P2: useful progress this week
- P3: nice-to-have / backlog

## 5. Source coverage and gaps
- Sources checked:
  - [[...]] — {{coverage_note}}
- Sources not checked:
  - {{source_or_none}}
- Gaps:
  - {{gap_or_none}}
- Confidence: {{high_medium_low}} — {{reason}}

## 6. Raw-note retention statement
Raw/source notes were retained unchanged. This weekly summary links to existing notes and does not assume deletion, consolidation, moving, or rewriting of source material.