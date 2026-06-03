# Wellness

This note is the human-readable wellness ledger. The dashboard is the shared source of truth for all structured food, training, weight, and macro data.

Related contract: [[Wellness Data Contract]]

## Source-of-truth contract

- Dashboard = canonical structured records and derived totals.
- `Wellness.md` = Obsidian projection plus manual narrative notes.
- Samson/dashboard sync may rewrite only generated blocks marked with `<!-- wellness:...:start -->` and `<!-- wellness:...:end -->`.
- Manual text outside generated blocks must be preserved.
- Manual structured edits in this note do not update the dashboard. To change food, training, weight, timestamps, macros, or deletion state, update the dashboard record first and re-render this note.

## Canonical rendering schema

Each generated event block should render these fields when available:

- `event_id`: canonical immutable id from the dashboard. If the dashboard has not exposed a canonical event id yet, use a stable temporary marker based on `source_system + source_record_id`, e.g. `dashboard:food:1`, and replace it with the canonical id on the next sync.
- `event_type`: `food`, `training`, `weight`, or `note`.
- `daily_key`: `YYYY-MM-DD` in `Europe/Athens`.
- `occurred_at`: ISO 8601 timestamp with timezone when available.
- `revision`: canonical dashboard revision when available.
- `status`: `active`, `superseded`, or `deleted`.
- payload summary: description/session/weight plus normalized structured values.

Recommended event marker format:

```text
<!-- wellness:event:{event_id}:start -->
... rendered event content ...
<!-- wellness:event:{event_id}:end -->
```

When only dashboard row ids are available, use this temporary source-record marker:

```text
<!-- wellness:source:{source_system}:{event_type}:{source_record_id}:start -->
... rendered event content ...
<!-- wellness:source:{source_system}:{event_type}:{source_record_id}:end -->
```

## Update and merge rules

### Normal sync path

1. User logs food/training/weight through Samson or the dashboard.
2. Dashboard stores or updates the canonical record.
3. `Wellness.md` is regenerated from dashboard data.
4. Sync replaces only the matching generated block between markers.
5. Daily rollups are recalculated from dashboard totals, not manually summed in Obsidian.

### Manual edits

Manual edits are accepted only as prose outside generated blocks, for example:

- reflections about energy, hunger, mood, sleep, soreness, motivation;
- extra context that does not change structured values;
- Obsidian links and local organization.

Manual edits are rejected or overwritten when they change structured fields inside generated blocks, including:

- food descriptions, calories, protein, carbs, fat;
- training activity type, timestamp, duration, intensity, or session subtype;
- weight values;
- status/deletion/supersession state;
- generated daily totals.

If a manual note appears to request a structured correction, the sync process must not silently merge it. It should create a dashboard review/correction action and leave the Obsidian prose intact until the dashboard record is changed.

### Conflict resolution

- Match records by `event_id` first.
- If no `event_id` exists, match by `source_system + source_record_id`.
- If still ambiguous, match by `event_type + occurred_at + normalized payload hash`.
- If ambiguity remains, do not guess; mark for manual review.
- Dashboard wins for structured-field conflicts.
- Obsidian wins for prose/layout outside generated blocks.

## Dashboard projection

### 2026-06-03

<!-- wellness:daily:2026-06-03:start -->
- Training: none.
- Food totals: 2495 kcal, 119g protein, 213g carbs, 123g fat.
- Goals: 2400 kcal, 180g protein.
- Remaining: -95 kcal, 61g protein.
<!-- wellness:daily:2026-06-03:end -->

<!-- wellness:source:mcp:food:2026-06-03:start -->
- Event type: food
- Daily key: 2026-06-03
- Source: dashboard / mcp meal log
- Items: omelete me 4 auga kai cottage, 1 agouraki, ntomata, ligo sauce me giaourti, ladi, moustarda, sriratsa, kai misi araviki pita
- Totals: 635 kcal, 43g protein, 35g carbs, 36g fat
- Status: active
<!-- wellness:source:mcp:food:2026-06-03:end -->

<!-- wellness:source:mcp:food:2026-06-03:snack-15:start -->
- Event type: food
- Daily key: 2026-06-03
- Source: dashboard / mcp meal log
- Items: ena bowl pagoto xiropito pagoto anana
- Totals: 360 kcal, 6g protein, 50g carbs, 14g fat
- Status: active
<!-- wellness:source:mcp:food:2026-06-03:snack-15:end -->

<!-- wellness:source:mcp:food:2026-06-03:meal-16:start -->
- Event type: food
- Daily key: 2026-06-03
- Source: dashboard / mcp meal log
- Items: 2 souvlakia: 1 gyro kotopoulo kai 1 gyro choirino, to ena me patates
- Totals: 1500 kcal, 70g protein, 128g carbs, 73g fat
- Status: active
<!-- wellness:source:mcp:food:2026-06-03:meal-16:end -->

<!-- wellness:source:mcp:training:2026-06-03:start -->
- Event type: training
- Daily key: 2026-06-03
- Source: dashboard / mcp training summary
- Activities: none
- Status: active
<!-- wellness:source:mcp:training:2026-06-03:end -->

## Manual notes

Manual notes below are preserved by sync and are not canonical structured records unless explicitly entered into the dashboard.

### Legacy notes before dashboard projection

- Weight update: 81.8kg on 2026-06-01
- Day 2 - 2026-06-01 10:13: Started workout; weight 81.8kg; Lat Pulldown 52kg | DB Row 20kg | Pullover 18kg | Seated Row 45kg.
- 2026-06-02 note: BJJ session completed; meals included chicken biftekia + salad + pita and half a Ninja Creami protein ice cream; good training day; no more food today; keep hydration high; latest tracked weight 81.8kg.
- 2026-06-03 supplement/order note: Protein powder order placed: 2x Immortal Whey Protein 2kg bundle (total 4kg), flavors Chocolate Peanut Butter + Raspberry Cheesecake, total €99.90, status processing. Personal delivery/contact details intentionally not logged here.
