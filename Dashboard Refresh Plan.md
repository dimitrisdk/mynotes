# Dashboard Refresh Plan

Date: 2026-06-03 09:18 UTC
Owner: Petros for implementation, Panagia for product direction/QA, Ioudas/Samson domain input as needed.

## Current findings

- Live dashboard is active at `http://100.107.122.62:9120`.
- Overview currently feels sparse: shortcuts + Hermes status only.
- Agents shown: Panagia, Petros, Samson, Ioudas. Missing: Solomon.
- Money UI exists in client route `/money`, but live API returns `HTTP 404`, so production route/deploy/restart needs verification.
- News tab is text-heavy; article cards need thumbnails/images or better placeholders.
- Fitness and Training are split; Dimitris wants one combined wellness area.
- Console check on Overview produced no JS errors.

## MVP goal

Make the dashboard feel current and useful from the first screen, without overbuilding.

## Proposed Overview sections

1. **Today**
   - Date/greeting
   - compact status: latest meal/training, money totals, open tasks, news freshness

2. **Agents**
   - Panagia, Petros, Samson, Ioudas, Solomon
   - state: running/telegram connected/model if available
   - quick link/button target where safe

3. **Money**
   - current month expenses
   - current month investments
   - latest entries
   - fix `/api/money` live route first

4. **Wellness**
   - combine Fitness + Training into one tab/section
   - food/macros, weight, meal photo, training streak/log buttons

5. **News**
   - top 3 tech/AI + top 3 Greek headlines
   - article thumbnails via RSS media/enclosure/og:image where reliable
   - fallback: source favicon or gradient placeholder

6. **Kanban**
   - open/running/blocked/done summary
   - highlight blocked tasks needing user approval

7. **System**
   - VPS RAM/disk/load
   - backup status
   - service status

8. **System map / diagrams**
   - simple architecture diagram: Telegram → Agents → Dashboard → Obsidian/Home Assistant/GitHub/News
   - use static SVG/mermaid-style card for MVP

9. **Usage / tokens**
   - investigate reliable Hermes token/source logs
   - show daily/weekly usage only if data is real
   - otherwise show “usage source not available yet” and document next step

## Shortcuts to add

- Money
- Wellness
- Kanban
- Obsidian TODO / vault
- Home Assistant
- Home Assistant All Off (only if safe/API-supported)
- Telegram agents: Panagia, Petros, Samson, Ioudas, Solomon
- News refresh
- GitHub
- Backup/system status

## MVP non-goals

- No bank integrations.
- No complex charts.
- No fake article images.
- No fake token counts.
- No broad design-system rewrite.

## Kanban

Created task: `t_4645b3d4` — Dashboard refresh MVP: overview, agents, news images, wellness section, shortcuts.
