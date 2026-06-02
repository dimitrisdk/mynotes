# Wellness Data Contract

## Purpose
This document defines the canonical schema and sync rules for wellness data shared between:
- Samson (the agent that captures and normalizes logs)
- The dashboard (structured storage and derived summaries)
- Obsidian `Wellness.md` (human-readable daily ledger)

The goal is to keep both sync directions unambiguous and idempotent.

## Core principle
The dashboard is the system of record for structured wellness events.
Obsidian `Wellness.md` is the human-facing projection of that data.
Samson is the writer/orchestrator, not the source of truth.

## Canonical record model
Every wellness event must map to one canonical record.

Required top-level fields:
- `event_id` — ULID or UUIDv7, globally unique and immutable
- `event_type` — one of `food`, `training`, `weight`, `note`
- `occurred_at` — ISO 8601 timestamp with timezone, in `Europe/Athens`
- `created_at` — ISO 8601 timestamp with timezone when the record first entered the system
- `updated_at` — ISO 8601 timestamp with timezone for the latest canonical edit
- `source_system` — one of `samson`, `dashboard`, `obsidian`
- `source_record_id` — the originating system’s primary key or stable anchor
- `revision` — integer, starts at 1 and increments on each canonical update
- `status` — one of `active`, `superseded`, `deleted`

Optional shared fields:
- `daily_key` — `YYYY-MM-DD` in `Europe/Athens`
- `title` — short human-readable label
- `notes` — free text
- `tags` — array of strings
- `confidence` — number from 0.0 to 1.0 for inferred values
- `estimated` — boolean, true when macros/values are estimated

## Event-specific schema

### 1) Food log
Required payload fields:
- `meal_type` — `breakfast`, `lunch`, `dinner`, `snack`, or `other`
- `description` — original food description in natural language

Recommended numeric fields:
- `kcal`
- `protein_g`
- `carbs_g`
- `fat_g`

Optional fields:
- `portion_text`
- `ingredients[]`
- `meal_context` — e.g. post-training, pre-training, late snack
- `estimated` — true if macros were inferred

### 2) Training log
Required payload fields:
- `activity_type` — `gym`, `bjj`, or `yoga`
- `session_summary` — short natural-language summary

Recommended fields:
- `duration_min`
- `intensity` — e.g. `low`, `moderate`, `hard`
- `performance_notes`
- `session_subtype` — e.g. `push`, `pull`, `legs`, `gi`, `no-gi`
- `belt_progress` — if relevant to BJJ
- `competition_result` — if relevant

### 3) Weight log
Required payload fields:
- `weight_kg`

Recommended fields:
- `measured_at`
- `measurement_context` — e.g. morning, fasted, post-training
- `body_fat_pct` — if available

### 4) Note / daily summary
Used for derived summaries only.
These records are rollups, not sources of truth for the underlying food/training/weight events.

## Authority by field

### Dashboard authoritative
The dashboard is authoritative for:
- `event_id`
- `revision`
- `status`
- `created_at`
- `updated_at`
- `source_record_id`
- all normalized structured fields:
  - calories and macros
  - activity type
  - session subtype
  - weight values
  - timestamps
  - deletion/supersession state

### Obsidian authoritative
Obsidian is authoritative for:
- markdown structure
- heading order
- prose summaries and narrative notes
- manual commentary outside auto-generated blocks
- wikilinks and local note organization

### Samson authoritative
Samson is authoritative for:
- parsing shorthand into structured fields
- deciding which canonical event type a user message maps to
- generating the first normalized record from user input

Samson is not authoritative for final structured values once the dashboard has stored them.

## Obsidian rendering contract
`Wellness.md` is a projection of canonical records, not the canonical store.

Rendering rules:
- One canonical event may be rendered in one or more daily sections, but it must keep the same `event_id` marker everywhere.
- Auto-generated blocks must be wrapped in hidden markers so they can be replaced safely on re-sync.
- Manual text outside the auto-generated block must never be deleted by sync.

Recommended marker format:
- `<!-- wellness:event:{event_id}:start -->`
- `<!-- wellness:event:{event_id}:end -->`

If a block is regenerated, the sync engine replaces only the content between those markers.

## Current note mapping
The existing `Wellness.md` patterns map like this:

- `Weight update: 81.8kg on 2026-06-01`
  - canonical type: `weight`
  - render: latest weight summary line
  - source of truth: dashboard weight record

- `## Day 2 - 2026-06-01 10:13`
  - canonical type: `training`
  - render: gym session block
  - source of truth: dashboard training record

- `- Lat Pulldown: 52kg | DB Row: 20kg | Pullover: 18kg | Seated Row: 45kg`
  - canonical type: `training`
  - render: workout detail line
  - source of truth: dashboard training payload

- `## Fitness Dashboard — 2026-06-02`
  - canonical type: `note` or daily rollup
  - render: derived daily summary
  - source of truth: dashboard rollup, not manual prose

## Update semantics

### Create
- New user input creates a new canonical record with a new `event_id`.
- If the same user message is reprocessed, the same `source_record_id` must be used so the write is idempotent.

### Update
- Updates create a new `revision` for the same `event_id`.
- Structured field changes are applied in the dashboard first.
- Obsidian is then regenerated from the canonical record.

### Delete
- Deletions are soft deletes.
- Dashboard sets `status = deleted` and `updated_at`.
- Obsidian removes the rendered block on the next sync, unless a review/debug mode is explicitly enabled.

### Supersession
- If a newer record replaces an older one, the old record gets `status = superseded` and a `superseded_by` reference in the dashboard.
- Obsidian shows only the latest active record.

## Conflict resolution

### Structured fields
If dashboard and Obsidian disagree on a structured field, the dashboard wins.
Examples:
- weight value
- macros
- training type
- timestamps
- revision
- deletion state

### Prose and formatting
If dashboard and Obsidian disagree on prose or layout, Obsidian wins for the human-facing note, except inside auto-generated blocks.

### Duplicate events
If the same logical event appears twice:
1. Match by `event_id` if available
2. Else match by `source_system + source_record_id`
3. Else match by `event_type + occurred_at + normalized payload hash`
4. If still ambiguous, mark for manual review rather than guessing

### Concurrent edits
If two systems change the same structured field before sync:
- pick the record with the highest `revision`
- if revision ties, pick the highest `updated_at`
- if both still tie, dashboard wins and the loser is marked stale

## Idempotency rules
- Every ingestion write must be safe to repeat.
- The same source payload must not create duplicate events.
- Obsidian sync must only rewrite blocks with matching `event_id` markers.
- Manual text outside the markers must remain untouched.

## Practical implementation rule
If a value can be calculated or normalized, store it in the dashboard.
If a value is meant for humans to read or annotate, store it in Obsidian as prose.

## Summary
- Dashboard = authoritative structured data
- Obsidian = authoritative presentation and notes
- Samson = parser and writer
- `Wellness.md` = rendered daily view, not the source of truth
