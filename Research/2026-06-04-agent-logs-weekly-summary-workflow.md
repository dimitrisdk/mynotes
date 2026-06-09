---
title: Weekly Agent Logs summary workflow
status: completed
created: 2026-06-04 23:55 UTC
created_by: Solomon
tags:
  - agent-logs
  - obsidian
  - workflow
  - weekly-summary
source_scope:
  - Agent Logs/
destination_scope:
  - Agent Logs/Weekly/
  - Agent Logs/Indexes/
raw_log_policy: retain-raw-logs-unchanged
kanban_task: t_f8ab0881
artifact: /root/.hermes/kanban/workspaces/t_f8ab0881/weekly-agent-logs-summary-workflow.md
---

# Weekly Agent Logs summary workflow

Linked artifact: `/root/.hermes/kanban/workspaces/t_f8ab0881/weekly-agent-logs-summary-workflow.md`

## Συμπέρασμα

- Weekly summaries should be generated every Monday morning for the previous complete Europe/Athens week.
- Raw logs stay immutable: no deletion, no merging, no “processed” edits inside source logs.
- Clean outputs go to `Agent Logs/Weekly/`; processing manifests go to `Agent Logs/Indexes/`.
- Each summary should link back to raw source notes and project notes such as [[Dashboard Refresh Plan]], [[TODO]], [[IT-LOGS]], and [[RESEARCH-LOG]].
- The implementation-ready template is saved at [[Agent Logs/Weekly/_template-agent-weekly-summary]].

## Workflow spec

### Cadence

- Run weekly: Monday 07:30 Europe/Athens.
- Coverage window: previous Monday 00:00 through Sunday 23:59 Europe/Athens.
- Late runs still summarize the previous complete week.
- Ad-hoc runs should create catch-up or revision notes, not overwrite existing summaries.

### Source folders

Primary source:
- `Agent Logs/*.md`

Include:
- Agent operational logs.
- Cleanup/deletion manifests.
- Cron/reminder/digest setup notes.
- News brief generation logs when they record useful system behavior.

Exclude or down-rank:
- `Agent Logs/Weekly/`
- `Agent Logs/Indexes/`
- Pure brief content already captured in `Briefs/` unless operationally important.

### Destination folders

- `Agent Logs/Weekly/YYYY-Www-agent-summary.md`
- `Agent Logs/Indexes/YYYY-Www-processed-sources.md`

### Raw log retention rules

- Do not delete raw logs.
- Do not merge raw logs.
- Do not rewrite raw log entries to mark them processed.
- Keep cleanup/deletion manifests permanently as audit evidence.
- Store processing state only in separate index/manifest notes.

### Summary note sections

1. Executive summary.
2. Agent activity by person.
3. Project updates.
4. Vault changes.
5. Automations / cron / integrations.
6. Decisions and preferences captured.
7. Open loops / follow-ups.
8. Source coverage.
9. Raw log retention confirmation.

### Required frontmatter

```yaml
---
title: Agent Logs Weekly Summary YYYY-Www
week: YYYY-Www
period_start: YYYY-MM-DD
period_end: YYYY-MM-DD
timezone: Europe/Athens
status: final
created: YYYY-MM-DD HH:mm UTC
created_by: Solomon
source_folder: Agent Logs/
source_files:
  - Agent Logs/Panagia.md
agents:
  - Panagia
projects:
  - Dashboard
people: []
tags:
  - agent-logs
  - weekly-summary
  - obsidian
raw_logs_retained: true
---
```

## Cleanup checklist

Before summary:
- [ ] Identify previous complete week in Europe/Athens.
- [ ] List candidate source files under `Agent Logs/`, excluding `Weekly/` and `Indexes/`.
- [ ] Read all candidate files changed during or after the target week.
- [ ] Search related notes if log entries mention them.
- [ ] Check [[TODO]], [[IT-LOGS]], [[RESEARCH-LOG]], and [[Dashboard Refresh Plan]].

During summary:
- [ ] Extract material outcomes, decisions, automations, vault changes, and blockers.
- [ ] Group items by agent and project.
- [ ] Add source links and line references where feasible.
- [ ] Convert noisy wording into concise operational summaries.
- [ ] Add follow-ups as explicit checklist bullets.
- [ ] Avoid secrets/tokens.

After summary:
- [ ] Create/update the weekly summary note.
- [ ] Create a processing manifest under `Agent Logs/Indexes/`.
- [ ] Confirm raw logs remain in place unchanged.
- [ ] Run `git status` in `/root/Documents/ObsidianVault/`.
- [ ] Create specialist kanban follow-ups if needed.

## Template

See [[Agent Logs/Weekly/_template-agent-weekly-summary]].

## Τι σημαίνει για τον Δημήτρη

Ο Δημήτρης θα βλέπει μία καθαρή εβδομαδιαία εικόνα του τι έκαναν οι agents, χωρίς να χαθεί το audit trail των raw logs. Το workflow είναι ασφαλές για automation επειδή κρατά τα raw logs ανέπαφα και βάζει όλη την “επεξεργασία” σε ξεχωριστά weekly/index notes.
