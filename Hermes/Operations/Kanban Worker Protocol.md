# Hermes Kanban — Worker Protocol

Purpose: every agent that picks up a Kanban task must follow this protocol. No more ad-hoc replies, no more protocol violations.

## 1. Claim
- Use `kanban_claim` (or `hermes kanban claim`) on a task that is `ready`.
- Do not assume ownership without claiming.
- If you are already implicitly assigned, still call `claim` once at the start.

## 2. Run book
For every task, before marking done, emit exactly these artifacts:
- **Status line:** what is happening / what blocked you.
- **Log updates:** short, factual notes, numbered if needed.
- **Verification:** how you confirmed the result (command output, URL, file content, screenshot, etc.).

Do not:
- write nothing and then call `complete`
- silently change strategy and disappear

## 3. Comment policy
- Use `kanban_comment` for every meaningful progress step.
- At minimum: comment once after claiming, once before completing, and any intermediate step that materially changes the approach or outcome.
- Format: short human-readable lines, not long walls of text.

## 4. Completion rules
Before calling `kanban_complete` you must have:
- performed the requested work
- added a final comment with the result, evidence, and any caveats
- not left the board in an inconsistent state

If a task cannot be completed because of missing info, missing permissions, or tooling, do NOT complete it. Either:
- unblock / depart it with a clear comment, или
- block it with a reason

## 5. Escalation rules
Escalate to Dimitris only when:
- approval is required for an irreversible action (restart, deploy, delete)
- the task is blocked by an external dependency
- the task is ambiguous and guessing would be worse than asking

Do not escalate for:
- normal variations in implementation
- optional style choices
- minor local decisions

## 6. Agent roster
Current canonical agents:
- **Panagia** — orchestrator / user-facing assistant
- **Petros** — code / dashboard / infra
- **Solomon** — research
- **Ioudas** — finance
- **Samson** — wellness / fitness
- **default (Hermes)** — fallback / orchestration

When a task is unclear, prefer reassignment to a specialist agent via `kanban_assign` rather than guessing.

## 7. Task template (copy/paste when creating tasks)
```md
Title: <short action>

Body:
- Goal: <one sentence>
- Assignee: <agent or leave unassigned>
- Workspace: <custom path or default>
- Priority: <low / medium / high>
- Verification steps:
  1. ...
  2. ...
- Expected output: <file / message / state change / link>
```
