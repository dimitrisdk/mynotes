# Scheduled jobs UI contract

Date: 2026-06-05 00:11 UTC
Kanban: `t_6004b28d`
Artifact: `/root/.hermes/kanban/workspaces/t_6004b28d/scheduled-jobs-ui-contract.md`

## Συμπέρασμα

- The dashboard should wrap Hermes cron jobs rather than invent a separate scheduler model.
- Minimum fields: id/name, schedule expression + display cadence, next/last run, last result/status, enabled/paused state, latest error summary, and alert policy.
- Dimitris preference is explicit product behavior: successful maintenance/backups stay silent by default; only failures/actionable errors surface alerts.
- Current backend lacks explicit `alert_policy` and first-class `silent` last result; MVP can derive both, but backend should add explicit metadata later.

## Facts

Audited implementation:

- `/usr/local/lib/hermes-agent/cron/jobs.py`
  - jobs stored in `${HERMES_HOME}/cron/jobs.json`.
  - outputs stored under `${HERMES_HOME}/cron/output/{job_id}/{timestamp}.md`.
  - job records include `id`, `name`, `schedule`, `schedule_display`, `next_run_at`, `last_run_at`, `last_status`, `last_error`, `last_delivery_error`, `enabled`, `state`, `paused_at`, `paused_reason`, `repeat`, `deliver`, `script`, `no_agent`, `skills`, `workdir`, `profile`.
- `/usr/local/lib/hermes-agent/tools/cronjob_tools.py::_format_job()` exposes the current public list shape: `job_id`, `name`, `schedule`, `next_run_at`, `last_run_at`, `last_status`, `last_delivery_error`, `enabled`, `state`, pause fields, and optional execution fields.
- `/usr/local/lib/hermes-agent/cron/scheduler.py` defines `SILENT_MARKER = "[SILENT]"` and skips delivery when successful output contains it.
- `no_agent=true` script jobs treat empty stdout or `wakeAgent=false` as silent success; non-zero exit/timeout becomes an error alert.
- `mark_job_run()` records `last_status`, `last_error`, `last_delivery_error`, `last_run_at`, next run, and state.
- Delivery errors are tracked separately from job execution errors.

## Contract summary

Primary endpoint proposed:

`GET /api/agent-os/scheduled-jobs`

Top-level response:

```json
{
  "ok": true,
  "generatedAt": "ISO timestamp",
  "source": {
    "kind": "hermes-cron",
    "profile": "solomon",
    "schedulerRunning": true,
    "schedulerStatus": "running|not_running|unknown",
    "permission": "read_only|manage|denied"
  },
  "jobs": [],
  "warnings": []
}
```

Each job should expose:

- `id`, `name`, optional safe `description`/`prompt_preview`.
- `schedule.expression`, `schedule.display`, `schedule.kind`, optional `timezone`.
- `timing.createdAt`, `timing.nextRunAt`, `timing.lastRunAt`, `timing.isStale`, `timing.staleReason`.
- `status.state`, `status.enabled`, `status.paused`, `status.lastResult`, `status.latestErrorSummary`, `status.severity`.
- `alerting.alertsErrorsOnly`, `alerting.successSilentByDefault`, `alerting.willNotifyOnSuccess`, `alerting.willNotifyOnFailure`, `deliverRaw`.
- `execution.mode`, `script`, `skills`, `profile`, `workdir`, `enabledToolsets`, `model`, `provider`, `repeat`.
- `actions.canRunNow`, `canPause`, `canResume`, `canEdit`, `canRemove`.

## UI states

- Loading: skeleton rows; no stale/error banners until API responds.
- Empty: `No scheduled jobs yet.` plus note that maintenance/backups stay quiet unless attention is needed.
- Normal: show job, cadence, next run, last run/result, state, alerts badge `Errors only`, and actions.
- Stale: banner if scheduler not running/unknown, active next run is overdue beyond grace, active job lacks `nextRunAt`, or cache is old.
- Permission denied: locked state, no job names.
- Jobs file unreadable/malformed: safe error with code, no secrets/absolute paths unless admin mode.
- Delivery config missing: actionable warning, not a success.

## Alert semantics

Dimitris preference to encode:

Successful maintenance/backups must be silent by default. Only failures, delivery failures, stale schedules, missing scheduler, or other actionable errors should surface alerts.

Implications:

- `alertsErrorsOnly=true` by default for maintenance/backups/watchdogs/sync/cleanup.
- Successful runs update dashboard state only; no green success notification feed.
- Silent no-agent success is healthy/quiet, not missing output.
- Script failure and delivery failure must alert.
- Future explicit field: `alertPolicy: "errors_only" | "every_run" | "silent"`.

## Gaps

- No stored `alert_policy` today.
- Public cron tool list omits `last_error`; backend should use `cron.jobs.list_jobs(include_disabled=true)` for dashboard.
- Silent success is inferred from output/marker, not stored as first-class `last_result`.
- Scheduler health is currently inferred from gateway PID; first-class scheduler heartbeat would be better.

## Τι σημαίνει για τον Δημήτρη

The dashboard can show scheduled maintenance/backups transparently without becoming noisy. Healthy jobs should be visible as rows/state, while alerts remain reserved for failures or things Dimitris must act on.
