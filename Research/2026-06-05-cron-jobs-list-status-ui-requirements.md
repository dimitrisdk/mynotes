# Cron jobs list and status UI requirements

Date: 2026-06-05 07:43 UTC
Kanban: `t_f0b1cf43`
Artifact: `/root/.hermes/kanban/workspaces/t_f0b1cf43/cron-jobs-list-status-ui-requirements.md`
Related: [[Research/2026-06-05-scheduled-jobs-ui-contract]]

## Συμπέρασμα

- Build the scheduled-tasks UI over Hermes cron, not a separate scheduler abstraction.
- The list must show: job identity, cadence, next run, last run/result, enabled/paused state, latest error, and alert policy.
- Dimitris’s constraint is explicit UX: successful maintenance/backups/watchdogs should be silent by default; only failures/actionable states should alert.
- MVP can derive `errors_only` and `silent_ok`, but durable backend fields for `alert_policy` and last silent result would reduce ambiguity.

## Facts

Audited sources:

- `/usr/local/lib/hermes-agent/cron/jobs.py`
  - stores jobs in `${HERMES_HOME}/cron/jobs.json`.
  - records `id`, `name`, `schedule`, `schedule_display`, `repeat`, `enabled`, `state`, `paused_at`, `paused_reason`, `created_at`, `next_run_at`, `last_run_at`, `last_status`, `last_error`, `last_delivery_error`, `deliver`, `script`, `no_agent`, `skills`, `enabled_toolsets`, `workdir`, `profile`.
  - `mark_job_run()` updates last status/error, delivery error, last/next run, and terminal states.
- `/usr/local/lib/hermes-agent/tools/cronjob_tools.py`
  - public list formatter exposes `job_id`, `name`, schedule, next/last run, `last_status`, `last_delivery_error`, enabled/state, pause fields, and optional execution fields.
  - current public formatter does not expose `last_error`.
- `/usr/local/lib/hermes-agent/cron/scheduler.py`
  - `SILENT_MARKER = "[SILENT]"` suppresses delivery while still saving output.
  - `no_agent=true` script jobs: non-empty stdout delivers; empty stdout / `wakeAgent=false` is silent success; non-zero exit / timeout alerts as failure.
- Live `cronjob(action='list')` for Solomon currently returns zero jobs, so UI examples are contract examples, not live Dimitris job data.

## Implementation-ready contract

Endpoint: `GET /api/agent-os/cron-jobs`

Each job should include:

- `id`, `name`, optional safe `description`/`promptPreview`.
- `schedule.display`, `schedule.expression`, `schedule.kind`, optional `timezone`.
- `timing.createdAt`, `timing.nextRunAt`, `timing.lastRunAt`, `timing.isOverdue`, `timing.overdueReason`.
- `status.state`, `status.enabled`, `status.paused`, `status.lastResult`, `status.latestErrorSummary`, `status.severity`.
- `alerting.policy`, `alertsErrorsOnly`, `successSilentByDefault`, `willNotifyOnSuccess`, `willNotifyOnFailure`, `deliverRaw`.
- `execution.mode`, `script`, `skills`, `profile`, `workdir`, `model`, `provider`, `repeat`.
- `actions.canRunNow`, `canPause`, `canResume`, `canEdit`, `canRemove`, `disabledReason`.

Recommended result values:

- `lastResult`: `ok | silent_ok | error | delivery_error | empty_response | never_run | unknown`.
- `state`: `scheduled | paused | completed | error | unknown` plus frontend-derived `stale` badge.
- `alerting.policy`: `errors_only | every_run | silent`; default to `errors_only` for maintenance/watchdog/backup/sync jobs.

## UI interactions

- Detail panel: full job metadata, separate execution vs delivery error, latest output link/summary, safe delivery target display.
- Run now: queue for next scheduler tick; disable duplicate clicks until refresh.
- Pause/resume: keep paused jobs visible; optional reason for later.
- Edit: name/schedule/alert policy first; prompt/script/skills only in advanced mode.
- Remove: destructive confirmation with job name/id.
- View output: show saved output when available; for silent successes say no alert was sent by design.

## UI states

- Loading: skeleton rows; no stale/error banners until response arrives.
- Empty: “No scheduled jobs yet. Maintenance/watchdog jobs can run quietly here; successful runs stay silent unless you choose otherwise.”
- Permission denied: locked state; do not leak job names/counts.
- Scheduler not running/unknown: warning banner; keep readable rows visible.
- Malformed/unreadable jobs file: safe error copy, retry action, no secrets/raw paths in normal mode.
- Job error: row enters `Needs attention`; truncate `latestErrorSummary` in list, full details only in detail/admin view.
- Stale/overdue: active job with overdue/missing `nextRunAt`, stale cache, or unknown scheduler should be visible as warning.

## Alert semantics

Default product rule:

Successful maintenance/backups/watchdogs/syncs stay silent. Alerts are for failures, stale schedules, missing scheduler, delivery failures, or other action needed.

Implications:

- `silent_ok` is healthy, not suspicious.
- Empty stdout in a successful `no_agent=true` job is healthy.
- `wakeAgent=false` is a healthy skip/silent signal.
- Non-zero script exit, timeout, prompt-injection block, provider/auth failure, empty agent response, schedule computation failure, and delivery failure are attention states.
- Do not show success toasts/notifications for every maintenance run.

## Acceptance criteria

- API returns all jobs including paused/disabled/completed/error jobs.
- UI renders loading, empty, permission, scheduler-stale, global API error, job error, and normal states.
- UI shows next run, last result, enabled/paused state, latest error, and alert policy per job.
- Execution errors and delivery errors are separate.
- Default alert policy is errors-only/silent-success for maintenance jobs.
- `Run now`, `Pause/Resume`, `Edit`, `Remove`, and `View latest output` are either implemented or disabled with explicit `disabledReason`.

## Assumptions / Gaps

- No explicit stored `alert_policy` exists yet; derive for MVP.
- No first-class stored `silent_ok`; infer from output marker/saved output or add backend persistence later.
- Scheduler health source is unspecified; likely needs gateway status or a cron heartbeat endpoint.
- Dashboard permission model is assumed to be `read_only | manage | denied`.

## Τι σημαίνει για τον Δημήτρη

The UI should make automation observable without training Dimitris to ignore noise. A quiet row with `Silent OK` is success; alerts are reserved for “look now” conditions.
