# Agent status and activity data inventory

Kanban task: `t_acbae261`
Generated: 2026-06-05 07:48 UTC
Artifact: `/root/.hermes/kanban/workspaces/t_acbae261/agent-status-activity-data-inventory.md`

## Summary

This note records the source-driven inventory for dashboard fields covering agent/profile status, gateway state, activity, routing, tasks, and logs.

## Key findings

- Enough data exists today for a practical per-agent dashboard card: gateway online/offline, PID, runtime state, per-platform state, routing destinations, assigned task counts, running task/run heartbeat state, recent session/message activity, and log mtimes.
- Best sources differ by field:
  - gateway liveness: per-profile `gateway.pid` validated by `gateway/status.py:get_running_pid()` plus `gateway_state.json`.
  - platform state: `gateway_state.json.platforms`.
  - routing: `channel_directory.json` plus platform config `home_channel`.
  - assigned/running work: `/root/.hermes/kanban.db` (`tasks`, `task_runs`, `task_events`).
  - last activity: profile `state.db.messages.timestamp`; fallback to log mtimes/session metadata.
- Existing dashboard APIs are profile-local. A fleet-wide Agents dashboard needs a new aggregator endpoint that iterates profiles and combines profile-local files with global Kanban DB data.

## Requested dashboard fields mapped

| Field | Source | Status |
|---|---|---|
| online/offline | `gateway.pid` validated by `get_running_pid()` | available |
| gateway status | `gateway_state.json.gateway_state` | available |
| platform status | `gateway_state.json.platforms` | available |
| last activity | max `state.db.messages.timestamp` | available, aggregation needed |
| assigned tasks | `kanban.db.tasks` grouped by assignee/status | available |
| running workers | `kanban.db.task_runs` + heartbeat fields | available |
| recent logs | `<profile>/logs/*` mtimes/tails | available, needs safe API/redaction |
| routing destination/state | `channel_directory.json`; config home_channel via `/api/messaging/platforms` | available/partial |
| fleet-wide API | proposed `GET /api/agents` or plugin route | gap |

## Sources inspected

- `/usr/local/lib/hermes-agent/gateway/status.py`
- `/usr/local/lib/hermes-agent/gateway/run.py`
- `/usr/local/lib/hermes-agent/gateway/channel_directory.py`
- `/usr/local/lib/hermes-agent/hermes_cli/web_server.py`
- `/usr/local/lib/hermes-agent/plugins/kanban/dashboard/plugin_api.py`
- `/root/.hermes/profiles/*/{gateway_state.json,channel_directory.json,state.db,logs/}`
- `/root/.hermes/kanban.db`

## Practical implication

Build the Agents dashboard as a profile-aware aggregator. Do not collapse all concepts into one “online” badge: gateway online, platform connected, currently working, and last activity are separate signals.
