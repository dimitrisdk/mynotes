# Solomon Research Tab Content Model

Date: 2026-06-05 07:43 UTC
Task: `t_7f192f06`
Status: Completed content model / implementation input
Scope: dashboard Research tab backed by `/root/Documents/ObsidianVault/RESEARCH-LOG.md` and `Research/*.md`

## Συμπέρασμα

- The Research tab should treat each displayed card as a real `ResearchEntry`, not UI-only text. Source of truth remains Obsidian markdown: `RESEARCH-LOG.md` is the index; `Research/*.md` carries the fuller note.
- Required fields are small: stable id, timestamp/date, title/topic, status, author, note path/existence, summary, source count, tags/topics, and watchlist relation.
- Optional fields enrich later views: source objects, confidence, kanban task id, artifact paths, freshness/review dates, related notes, open questions, and draft/final metadata.
- Watchlists should be first-class group/filter records, not hard-coded chips only. They can be populated by explicit note frontmatter and/or keyword matching.
- Avoid fake metrics/cards. If a field is not known, display `unknown`, `pending`, or empty state rather than inventing counts.

## Facts from current implementation/context

- Current Obsidian source of truth:
  - Log: `/root/Documents/ObsidianVault/RESEARCH-LOG.md`
  - Notes: `/root/Documents/ObsidianVault/Research/`
- Existing dashboard parser already reads log rows with columns: `Date`, `Topic`, `Status`, `Notes`.
- Existing API enriches linked notes from safe `Research/*.md` wikilinks only.
- Existing enriched entry fields include: `id`, `date`, `topic`, `status`, `statusKind`, `notes`, `wikilink`, `notePath`, `noteUrl`, `exists`, `summary`, `sourceCount`, `sources`, `tags`.
- Existing watchlist topics: AI, Hermes, UX, ETFs, BJJ.
- Current UI sections already map to: recent research notes, sources/links, draft/final status, and topic watchlists.

## Entities

### 1. ResearchEntry

A single research note/card shown in the Solomon Research tab.

Required fields:

| Field | Type | Required | Purpose / display |
| :--- | :--- | :---: | :--- |
| `id` | string | yes | Stable unique key for API/UI. Prefer slug from timestamp + topic or frontmatter `id`. |
| `title` / `topic` | string | yes | Card title and search target. |
| `createdAt` | ISO datetime or log datetime string | yes | Sort and display recency. Derived from log date if no frontmatter. |
| `updatedAt` | ISO datetime or null | yes | Show freshness; null means unknown. |
| `status` | enum | yes | Raw display status: `draft`, `final`, `completed`, `pending`, `archived`, `needs-review`. |
| `statusKind` | enum | yes | Normalized UI grouping: `draft`, `final`, `other`, `unknown`. |
| `author` | string | yes | Usually `solomon`; future-proof for Panagia/Petros coauthored notes. |
| `notePath` | string or null | yes | Safe relative path such as `Research/2026-06-05-example.md`; null if only log row exists. |
| `exists` | boolean | yes | Whether `notePath` resolves to an actual markdown note. |
| `summary` | string[] | yes | 0–4 bullets from `## Συμπέρασμα` / `## Conclusion`; empty array allowed. |
| `sourceCount` | integer | yes | Count of URLs under `## Πηγές` / `## Sources`; 0 if none. |
| `tags` | string[] | yes | Normalized tags without `#`. Empty array allowed. |
| `topics` | string[] | yes | Semantic topics/tags, e.g. `ai`, `hermes`, `dashboard`. Can initially mirror tags/watchlists. |
| `watchlistIds` | string[] | yes | Explicit or inferred membership: `ai`, `hermes`, `ux`, `etfs`, `bjj`. Empty array allowed. |

Recommended required frontmatter in new notes:

```yaml
---
id: research-2026-06-05-example
status: draft|final|completed|needs-review
author: solomon
created: 2026-06-05T07:43:00Z
updated: 2026-06-05T07:43:00Z
tags: [research]
topics: [ai, hermes]
watchlists: [ai, hermes]
confidence: medium
---
```

Fallback rules when frontmatter is missing:

- `id`: generate from log `date + topic`.
- `createdAt`: use log `Date` cell.
- `status`: use log `Status` cell.
- `author`: default to `solomon` for entries in `RESEARCH-LOG.md` unless `Author:` or frontmatter says otherwise.
- `summary`: parse first bullets from `## Συμπέρασμα` or `## Conclusion`; otherwise use the log `Notes` text as a short fallback.
- `topics/watchlistIds`: prefer frontmatter; otherwise infer from tags and watchlist keywords.

### 2. SourceLink

A cited URL or local artifact supporting a research note.

Required fields:

| Field | Type | Required | Purpose / display |
| :--- | :--- | :---: | :--- |
| `id` | string | yes | Stable key, e.g. hash of `entryId + url/path`. |
| `entryId` | string | yes | Parent ResearchEntry. |
| `label` / `title` | string | yes | Human-readable link label. |
| `kind` | enum | yes | `web`, `paper`, `docs`, `repo`, `video`, `pdf`, `local-artifact`, `obsidian-note`, `other`. |
| `url` | string or null | yes | HTTP(S) link when available. |
| `path` | string or null | yes | Local/vault artifact path when relevant. |
| `capturedAt` | datetime or null | yes | When Solomon used/captured it; null if unknown. |

Optional fields:

- `publisher`, `author`, `publishedAt`, `accessedAt`
- `credibility`: `primary`, `credible-secondary`, `community`, `unknown`
- `excerpt` / `whyItMatters`
- `archivedCopyPath` if a raw extract is stored later under `Research/Sources/`

Display:

- The card needs only source count and maybe top 1–3 links.
- A source drawer/detail page can show all links with credibility and excerpts.

### 3. Watchlist

Persistent monitored topic area for Solomon.

Required fields:

| Field | Type | Required | Purpose / display |
| :--- | :--- | :---: | :--- |
| `id` | string | yes | `ai`, `hermes`, `ux`, `etfs`, `bjj`. |
| `label` | string | yes | Display chip title. |
| `description` | string | yes | What the watchlist covers. |
| `keywords` | string[] | yes | Inference/search terms. |
| `entryCount` | integer | yes | Number of matching notes in the current result set. |
| `latestEntryIds` | string[] | yes | Most recent matching entries. |
| `status` | enum | yes | `active`, `paused`, `empty`, `needs-seed`. |

Optional fields:

- `owner`: default `solomon`
- `cadence`: `manual`, `daily`, `weekly`, `monthly`
- `lastCheckedAt`, `nextCheckAt`
- `sourceFeeds`: RSS/blog/search/API definitions if later connected to Blogwatcher/Firecrawl/Polymarket/arXiv
- `openQuestions`: unanswered questions Dimitris cares about for this watchlist

Watchlist relation rule:

- A `ResearchEntry` can belong to many watchlists.
- Explicit `watchlists:` frontmatter wins.
- Inference is allowed for search/filter convenience, but UI should mark it as inferred if shown in details.

### 4. ResearchQuery / Ask Solomon event (optional future)

If the search box `ρώτα τον Solomon` becomes more than local filtering, store lightweight query records.

Required if implemented:

- `id`, `query`, `createdAt`, `askedBy`, `mode`: `filter-only` or `research-request`
- `resultEntryIds`: notes returned/created
- `status`: `answered`, `queued`, `failed`, `no-match`

Recommendation: do not store local filter keystrokes. Store only submitted questions or generated research requests.

## Required fields for MVP API response

```ts
type ResearchEntry = {
  id: string;
  date: string;              // keep current field for compatibility
  createdAt: string;
  updatedAt: string | null;
  topic: string;
  status: string;
  statusKind: 'draft' | 'final' | 'other' | 'unknown';
  author: string;
  notes: string;
  wikilink: string | null;
  notePath: string | null;
  noteUrl: string | null;
  exists: boolean;
  summary: string[];
  sourceCount: number;
  sources: SourceLink[];
  tags: string[];
  topics: string[];
  watchlistIds: string[];
};

type ResearchResponse = {
  selectedTaskId: string;
  count: number;
  totalCount: number;
  statusSummary: Record<string, number>;
  recentNotes: ResearchEntry[];
  sourceLinks: SourceLink[];
  watchlists: Watchlist[];
  entries: ResearchEntry[];
};
```

MVP-compatible addition: keep current fields exactly and add `createdAt`, `updatedAt`, `author`, `topics`, `watchlistIds` without breaking existing UI.

## Optional fields for richer detail pages

- `confidence`: `low`, `medium`, `medium-high`, `high`
- `taskId`: kanban task id such as `t_7f192f06`
- `artifactPaths`: local generated files/specs/data exports
- `relatedNotePaths`: related Obsidian notes
- `relatedEntryIds`: similar/preceding research notes
- `decisionImpact`: `low`, `medium`, `high`
- `audience`: `dimitris`, `petros`, `panagia`, `team`, `personal`
- `language`: `el`, `en`, `mixed`
- `reviewedAt`, `reviewedBy`
- `expiresAt` / `staleAfter`: useful for prices, APIs, markets, model comparisons
- `openQuestions`: strings or structured records
- `recommendations`: strings or structured records
- `assumptions`: strings or structured records
- `facts`: sourced bullet IDs if the detail page later parses note sections

## Example records

### Example 1 — completed sourced note

```json
{
  "id": "2026-06-05-obsidian-vault-taxonomy-proposal",
  "date": "2026-06-05 02:57 UTC",
  "createdAt": "2026-06-05T02:57:00Z",
  "updatedAt": "2026-06-05T02:57:00Z",
  "topic": "Obsidian vault taxonomy proposal",
  "status": "Completed",
  "statusKind": "final",
  "author": "solomon",
  "notes": "Audited People/Finance/Health/Recipes/Research/Projects/Design/Agent Logs scope and preserved Keep Import categories; note: [[Research/2026-06-05-obsidian-vault-taxonomy-proposal]]; kanban `t_83991243`.",
  "wikilink": "Research/2026-06-05-obsidian-vault-taxonomy-proposal",
  "notePath": "Research/2026-06-05-obsidian-vault-taxonomy-proposal.md",
  "noteUrl": "/api/research/notes/Research%2F2026-06-05-obsidian-vault-taxonomy-proposal.md",
  "exists": true,
  "summary": [
    "The vault is already usable but uneven: Research, Finance, Agent Logs, and Keep Imports have recognizable homes; People, Projects, Design, Health, and Recipes are still partly root-level or mixed into Keep Imports/TODO.",
    "Keep Imports must remain protected.",
    "Recommended direction: keep logs and raw/imported material stable, add canonical top-level domains, then migrate by linking first."
  ],
  "sourceCount": 0,
  "sources": [],
  "tags": ["research", "obsidian", "taxonomy"],
  "topics": ["obsidian", "dashboard", "taxonomy"],
  "watchlistIds": ["hermes", "ux"]
}
```

### Example 2 — draft watchlist note

```json
{
  "id": "2026-06-06-ai-model-routing-watchlist",
  "date": "2026-06-06 09:00 UTC",
  "createdAt": "2026-06-06T09:00:00Z",
  "updatedAt": null,
  "topic": "AI model routing watchlist",
  "status": "Draft",
  "statusKind": "draft",
  "author": "solomon",
  "notes": "Draft watchlist entry: [[Research/2026-06-06-ai-model-routing-watchlist]]; tags: #ai, #model-routing; sources pending.",
  "wikilink": "Research/2026-06-06-ai-model-routing-watchlist",
  "notePath": "Research/2026-06-06-ai-model-routing-watchlist.md",
  "noteUrl": "/api/research/notes/Research%2F2026-06-06-ai-model-routing-watchlist.md",
  "exists": true,
  "summary": [],
  "sourceCount": 0,
  "sources": [],
  "tags": ["ai", "model-routing", "status/draft"],
  "topics": ["ai", "llm", "routing"],
  "watchlistIds": ["ai", "hermes"]
}
```

### Example 3 — log row with missing note

```json
{
  "id": "2026-06-06-bjj-recovery-sources",
  "date": "2026-06-06 18:30 UTC",
  "createdAt": "2026-06-06T18:30:00Z",
  "updatedAt": null,
  "topic": "BJJ recovery sources",
  "status": "Pending",
  "statusKind": "draft",
  "author": "solomon",
  "notes": "Queued note: [[Research/2026-06-06-bjj-recovery-sources]]; tags: #bjj, #recovery.",
  "wikilink": "Research/2026-06-06-bjj-recovery-sources",
  "notePath": "Research/2026-06-06-bjj-recovery-sources.md",
  "noteUrl": "/api/research/notes/Research%2F2026-06-06-bjj-recovery-sources.md",
  "exists": false,
  "summary": [],
  "sourceCount": 0,
  "sources": [],
  "tags": ["bjj", "recovery"],
  "topics": ["bjj", "recovery"],
  "watchlistIds": ["bjj"]
}
```

## Display rules

Recent research notes:

- Sort by `createdAt` descending, then `updatedAt` descending if needed.
- Show `topic`, `date`, `status`, 1–4 summary bullets or log note fallback, source count, top tags, and markdown link.
- If `exists=false`, show warning `linked note missing` but keep the log row visible.

Sources / links:

- Show `sourceCount` per entry.
- For source drawer/list, show `title`, `kind`, `publisher` if known, and URL/path.
- Do not imply credibility unless `credibility` is explicitly set or source kind is clearly primary docs/paper/repo.

Draft / final status:

- Use `statusKind` for counts.
- Raw `status` stays visible for nuance, e.g. `needs-review`, `pending restart/review`.
- `Completed` and `Final` both count as final.
- `Draft`, `Pending`, and `In progress` count as draft.

Tags/topics:

- Tags come from note frontmatter and `Tags:` lines.
- Topics are semantic and may be broader than tags.
- Keep tags normalized without `#` in API; add `#` only in UI presentation.

Authorship:

- Default author is `solomon`.
- Add `coauthors` optional array only when another agent/person materially contributed.
- Do not expose private profile paths or tokens in author metadata.

Timestamps:

- Store machine-readable ISO values where possible.
- Preserve the current human log date string for backwards compatibility.
- If only log date exists, parse best effort; otherwise display raw date and set ISO field null.

Watchlists:

- Watchlist chips should be clickable filters.
- Empty watchlists should show `needs seed` / `no notes yet`, not fake entries.
- Later scheduled monitoring can attach to watchlists through `sourceFeeds` and `cadence`.

## Assumptions / gaps

- Assumption: Obsidian remains the source of truth; the dashboard is a read-only projection for MVP.
- Assumption: `RESEARCH-LOG.md` remains markdown table based for now, even if frontmatter becomes richer in individual notes.
- Gap: no global Obsidian tag taxonomy is fully enforced yet.
- Gap: no decided storage for raw source extracts. Candidate: `Research/Sources/`.
- Gap: current UI filter is local text filtering; it is not yet a submitted Ask-Solomon research workflow.
- Gap: status vocabulary is not fully standardized across old log rows.

## Open questions / unresolved decisions

1. Should `RESEARCH-LOG.md` stay the canonical index long-term, or should the dashboard eventually scan all `Research/*.md` frontmatter and generate the log/index?
2. Should watchlists be edited in Obsidian frontmatter, in a dashboard settings JSON, or both?
3. Should source credibility labels be manually assigned by Solomon or inferred from source type/domain?
4. Should draft notes be visible by default, or should the default view show only final/completed with a draft toggle?
5. Should stale/currentness warnings be mandatory for market/product/API research with prices or versions?
6. Should `ρώτα τον Solomon` create kanban tasks for new research requests, or remain a search/filter box until explicitly submitted?
7. Should local artifact paths be visible in the UI, or only in note markdown for operator use?

## Recommendations

1. Implement this in two layers:
   - Compatibility layer: keep current log parser and API fields.
   - Enrichment layer: add frontmatter parsing for `author`, `created`, `updated`, `topics`, `watchlists`, `confidence`, `taskId`.
2. Add a small schema validator in the dashboard backend tests so malformed notes degrade gracefully rather than breaking `/api/research`.
3. Make watchlists clickable filters and include `entryCount`, `latest`, `status` from API instead of hard-coded frontend-only chips.
4. Keep draft notes visible, but visually distinct. Dimitris benefits from seeing in-progress research and missing-source warnings.
5. For future Ask-Solomon behavior, create a kanban research task only on explicit submit, not every search keystroke.

## Τι σημαίνει για τον Δημήτρη

- The Research tab becomes a reliable memory surface: what Solomon found, what sources back it, what is draft vs final, and which long-running watchlists are warm.
- The model avoids dashboard theater: no fake source counts, no invented research cards, no hidden provenance.
- Petros can implement the next dashboard iteration incrementally without breaking the current Obsidian-backed Researches tab.
