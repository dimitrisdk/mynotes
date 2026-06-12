# Research calendar and reminder patterns

Date: 2026-06-05
Kanban: t_39f68d0c
Status: Completed
Tags: #calendar #reminders #dashboard #product-research

## Συμπέρασμα

- Best pattern for a personal dashboard/calendar layer: one canonical day timeline plus separate low-noise reminder channels. Calendar events should be time blocks; tasks/renewals should be reminders or tasks with due dates, not fake all-day events unless visibility matters.
- Low-spam reminder design depends on per-calendar/list defaults, explicit overrides for high-risk items, and calendar-level muting for noisy subscribed/shared feeds. Apple Calendar, Google Calendar, Todoist, and Fantastical all expose some form of default/reminder override or calendar-specific alert control.
- Read-only integrations are usually handled by calendar subscription or share permissions: Google offers view-only/free-busy/share permissions and public URL add; Apple explicitly says subscribed calendars are controlled by the provider and cannot be edited; iCloud public calendars are read-only; Notion Calendar requires subscribing to external calendars via Google first.
- For Dimitris’s dashboard, implement read-only calendar ingestion first, not write-back. Show events/reminders from upstream sources, preserve source calendar/list metadata, and avoid generating extra notifications unless the item is high priority.

## Facts

### Product/system comparison

| System | What works for daily review | Reminder/noise controls | Read-only integration pattern | Caveats |
|---|---|---|---|---|
| Google Calendar | Multi-calendar day/week timeline; per-calendar default reminders; agenda notification type exists. | Reminders are alarms before event start; defaults are per calendar and user-specific; event-level overrides require `useDefault=false`; notification settings are per calendar. | Sharing supports permissions such as free/busy, see all details, make changes; public URL add is possible only for public calendars. | Subscribing to new Google calendars must be done in desktop browser; Google Help says you cannot subscribe to non-Google calendars from that flow. |
| Apple Calendar + Reminders | iOS/macOS Calendar can show scheduled Reminders directly in the calendar; timed reminders appear at their time in full-day schedule; all-day reminders appear at top. | Reminders support time alerts, location alerts, tags, flags; all-day reminders default to 9:00 AM but can be changed. Apple Calendar subscriptions can ignore alerts. | Calendar subscriptions are view-only; iCloud private sharing can be Read-Only; public iCloud calendar links are view-only. | Strong if user is Apple-first; weaker as universal backend unless iCloud/CalDAV access is explicitly handled. |
| Todoist | Good for renewals, subscriptions, training prep tasks, recurring responsibilities; natural-language recurring dates and Today view semantics. | Automatic reminders only when a task has date+time; automatic reminders can be disabled; custom reminders and snooze intervals exist; recurring task completion auto-shifts next due date. | Integrates as task source rather than calendar authority; can be surfaced into calendar/dashboard as tasks/reminders. | Multiple custom reminders require Pro/Business; complex recurrence formats have limitations. |
| Fantastical | Unified calendar+tasks client; DayTicker/day/week/month views; tasks can show alongside events; calendar sets make work/home review low-friction. | Default alerts for timed/all-day/birthdays; Time to Leave; can silence shared calendar changes/all-day tasks and ignore alerts per calendar. | Connects iCloud, Google, Exchange, Microsoft 365, CalDAV, Todoist etc.; shared/delegated calendars are managed per provider/account. | Product is a premium client, not a backend; best as UX inspiration unless the dashboard targets Apple/Windows client users. |
| Notion Calendar | Useful if Notion databases are central: date-property databases can be shown and edited in calendar; Notion Home has upcoming-events widget. | Less mature reminder/noise model in docs compared with Google/Apple/Todoist/Fantastical; no travel time. | Supports Apple iCloud and Google calendars; cannot subscribe to new external calendars inside Notion Calendar — subscribe via Google first. Notion databases are visible in Notion Calendar, not Google/iCloud. | Good for project/database dates, weaker for reliable notification layer or universal calendar ingestion. |

### Practical pattern by item type

- Today timeline: treat actual appointments/classes/meetings as events with start/end time; show current/next/upcoming. Avoid one undifferentiated list.
- Medical appointments: event in primary calendar plus explicit reminder override (e.g. day before + travel/time-to-leave) because missed cost is high.
- Renewals/subscriptions: recurring task/reminder rather than calendar event; show in dashboard under upcoming obligations. Use one advance reminder only unless financial/health risk is high.
- Training classes: recurring calendar event if attendance is scheduled; add separate Todoist/Reminders task only for prep actions (pack gear, payment, booking window).
- Low-spam reminders: start from no or one default reminder per calendar/list; add overrides only for high-risk events; mute imported/shared/subscribed calendars by default; batch non-urgent items into daily review rather than push notifications.

## Assumptions / Gaps

- I did not test live APIs or app UIs; this is documentation-backed product/system research.
- Exact notification behavior varies by OS-level notification permissions, Focus/Do Not Disturb, app plan, and mobile/desktop platform.
- Notion Calendar’s current notification/reminder capabilities are less clearly documented than Google/Apple/Todoist/Fantastical, so treat it as a planning/project-date surface rather than the primary alarm system.

## Εκτίμηση

The safest architecture is hub-and-spoke:

1. Ingest Google/Apple/ICS/Todoist as read-only sources.
2. Normalize to `event`, `task/reminder`, `renewal`, `medical`, `training` categories.
3. Render a single Today timeline plus “Upcoming obligations”.
4. Let source apps own push notifications initially.
5. Add dashboard-generated reminders only as opt-in escalation rules, not as a second default alarm layer.

This avoids the classic failure mode: every source already notifies, then the dashboard also notifies, so the user learns to ignore everything.

## Product implications

- Data model should include: source provider, source calendar/list, read-only flag, start/end, all-day, recurrence summary, reminder policy, category, risk level, muted/visible state.
- UI should include: Today timeline, next appointment, upcoming renewals, upcoming classes, medical section, and a low-noise “needs attention” lane.
- Default notification stance: dashboard is silent unless item is explicitly marked critical, overdue, or missing confirmation.
- Read-only integrations should be first-class: display source attribution and disable edit buttons for subscribed/shared read-only calendars.
- For imported/subscribed calendars, default to `visible=true`, `notify=false`, with a per-source toggle.

## Πηγές

- Google Calendar API — Reminders & notifications: https://developers.google.com/workspace/calendar/api/concepts/reminders
- Google Calendar Help — Subscribe to someone else’s calendar: https://support.google.com/calendar/answer/37100?hl=en
- Apple Support — Use Reminders on iPhone/iPad: https://support.apple.com/en-us/102484
- Apple Support — Use reminders in Calendar on iPhone: https://support.apple.com/guide/iphone/use-reminders-iph14f1d32a5/ios
- Apple Support — Subscribe to calendars on Mac: https://support.apple.com/guide/calendar/subscribe-to-calendars-icl1022/mac
- Apple Support — Share a calendar on iCloud.com: https://support.apple.com/guide/icloud/share-a-calendar-mm6b1a9479/icloud
- Todoist Help — Introduction to reminders: https://www.todoist.com/help/articles/introduction-to-reminders-9PezfU
- Todoist Help — Introduction to recurring dates: https://www.todoist.com/help/articles/introduction-to-recurring-dates-YUYVJJAV
- Fantastical product page: https://flexibits.com/fantastical
- Fantastical for Mac Help — Settings: https://flexibits.com/fantastical/help/settings
- Notion Help — Manage calendars & events: https://www.notion.com/help/manage-your-calendars-and-events
- Notion Help — Use Notion Calendar with Notion: https://www.notion.com/help/use-notion-calendar-with-notion

## Τι σημαίνει για τον Δημήτρη

Build the dashboard as a calm review layer, not a new notification app. Let calendar/reminder apps keep their native alerts; the dashboard should mostly answer “what is today / what is coming / what must not be missed” and only escalate a small number of high-cost items like medical appointments, renewals before charge/cancellation windows, and confirmed training sessions.
