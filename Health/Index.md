---
title: Health Index
domain: health
status: active
created: 2026-06-05
created_by: Petros
source_policy: link-only-no-deletion
tags: [health, index, vault-structure]
---

# Health Index

Canonical index for wellness, training, medical, supplements, and health data.

## Sub-folders
- [[Health/Training/Index|Training]] — BJJ, yoga, gym, gymnastics programs
- [[Health/Medical/Index|Medical]] — appointments, providers, notes
- [[Health/Supplements/Index|Supplements]] — protein, supplements, orders
- [[Health/Data Contracts/Index|Data Contracts]] — sync contracts, schemas

## Current linked content (root)
*Links to existing notes until migration approval*

### Wellness (dashboard projection — **path-locked**, do not move until sync paths audited)
- `[[../Wellness]]` — human-readable wellness ledger/projection from dashboard
- `[[../Wellness Data Contract]]` — sync/data-contract note

### Training
- `[[../Gymnastics]]` — training/program content

### From Keep Imports
- `[[../Keep Imports/Πρόγραμμα Γυμναστηρίου (BJJ-Yoga)]]` — training program
- `[[../Keep Imports/Gym]]` — gym notes
- `[[../Keep Imports/Αντιφλεγμ]]` — medical
- `[[../Keep Imports/Odotiatros]]` — medical provider (also in People/Providers)
- `[[../Keep Imports/Ακτίνα]]` — possible medical

## Migration rules
- **Do not move** `Wellness.md` or `Wellness Data Contract.md` until dashboard/Samson sync paths are audited and updated
- Dashboard remains canonical for structured wellness records; Obsidian is projection/prose
- Keep generated `wellness:` HTML markers untouched
- Medical/provider notes may contain sensitive personal health data — handle per approval
- Reusable recipes stay in `Recipes/`; daily food logs link from Health/Wellness