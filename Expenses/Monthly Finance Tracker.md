# Monthly Finance Tracker

Purpose: simple monthly tracker for Dimitris' expenses and investments. This is the human-readable Obsidian view; the matching CSV structure is in [[Finance/finance-tracker|finance-tracker.csv]].

Source of truth: append every monthly entry using the schema below. Keep one row per transaction/contribution so monthly totals can be calculated later without redesign.

## Entry schema

Required fields:
- Date: transaction or investment date in YYYY-MM-DD.
- Month: YYYY-MM, derived from Date.
- Description: short human-readable label.
- Amount: positive numeric amount. Use Type to distinguish expense vs investment.
- Currency: e.g. EUR, USD.
- Type: Expense or Investment.
- Category / Asset: expense category or investment asset label.
- Account: payment card, bank, broker, or investment account if known.
- Notes: optional context.
- Source / Status: where the entry came from and whether it is pending, settled, manual, imported, or verified.

## Category taxonomy

Top-level types:
- Expense
- Investment

Initial expense categories:
- Wellness / Supplements: supplements, protein powder, vitamins, health/wellness purchases.
- Software / Subscriptions: SaaS and recurring digital tools.
- Food / Dining: restaurants, delivery, cafés.
- Convenience / Small Purchases: kiosks, small retail, miscellaneous small spend.
- Other Expense: temporary category only; rename to a clearer category when patterns emerge.

Initial investment asset labels:
- ETF Investment: generic ETF contribution when the specific ETF is not yet known.
- ETF - VWCE / Vanguard FTSE All-World UCITS ETF Acc: use when investment is specifically VWCE / IE00BK5BQT80.
- ETF - VUAA / Vanguard S&P 500 UCITS ETF Acc: use when investment is specifically VUAA / IE00BFMXXD54.
- ETF - EWSP / iShares S&P 500 Equal Weight UCITS ETF Acc: use when investment is specifically EWSP / IE000MLMNYS0.
- Other Investment: temporary label only; replace with the exact asset/ticker/ISIN when known.

Status guidance:
- pending: visible in account but not fully settled.
- settled: confirmed transaction.
- manual: manually entered without source document.
- imported: entered from broker/bank export.
- verified: checked against source document/export.

## Monthly entries

| Date | Month | Description | Amount | Currency | Type | Category / Asset | Account | Notes | Source / Status |
| :--- | :--- | :--- | ---: | :--- | :--- | :--- | :--- | :--- | :--- |
| 2026-06-03 | 2026-06 | Protein powder order | 99.90 | EUR | Expense | Wellness / Supplements | Card / bank app unknown | Initial tracked expense; source was June wallet screenshot | Wallet screenshot / pending |
| 2026-06-03 | 2026-06 | ETF investment contribution | 1300.00 | EUR | Investment | ETF Investment | Broker unknown | Initial tracked investment; specific ETF/account not yet confirmed | Manual note from user / manual |
| 2026-06-20 | 2026-06 | HQ test expense | 1.23 | EUR | Expense | Other Expense | test account | Temporary test row for expense tool verification | Manual / test |

## Intake flow for future messages to Ioudas

When Dimitris sends a new expense or investment, capture it immediately as one tracker row and mark it as recorded.

1. Identify the entry type:
   - Expense: money spent on consumption, subscriptions, food, wellness, services, etc.
   - Investment: ETF/broker contribution or asset purchase.
   - Transfer/unknown: do not force it into Expense or Investment; ask/mark missing info until clarified.
2. Capture required fields:
   - Date: use the transaction date if visible; otherwise use today's date and note that it is user-reported.
   - Month: YYYY-MM from Date.
   - Description: merchant/payee or short label.
   - Amount and Currency: positive amount only; Type explains whether it is expense or investment.
   - Type: Expense or Investment.
   - Category / Asset: choose the best existing category/asset; use Other Expense or Other Investment only temporarily.
   - Account: card, bank, broker, or "unknown" if not provided.
   - Notes: only useful context, not sensitive identifiers.
   - Source / Status: e.g. user message / manual, wallet screenshot / pending, bank export / verified, broker export / verified.
3. Categorize conservatively:
   - Reuse existing categories before creating a new one.
   - Mark uncertain categories as "inferred" in Notes.
   - If merchant meaning is unclear, record the row but add it to the missing-information list for the month.
4. Duplicate check before adding:
   - Compare date, amount, merchant/description, and source against existing entries for that month.
   - If it looks like the same transaction, update status/notes instead of adding a duplicate.
5. Mark as recorded:
   - In the tracker row, Source / Status must include one of: pending, settled, manual, imported, or verified.
   - In conversation, reply briefly: "Recorded: €X — Category — Status" and mention any missing fields.
6. Escalate only when needed:
   - Ask Dimitris for clarification only if category, amount, date, or type materially changes monthly totals or investment tracking.
   - For ETF investments, prefer exact broker, ticker, ISIN, amount, currency, date, and fees when available.

## Monthly cutoff and check-in process

Use calendar month cutoff.

1. Cutoff: last calendar day of the month at local end-of-day.
2. First monthly check: during the first 1-3 days of the next month.
3. Before summary:
   - Add/import all visible bank, card, and broker entries for the month.
   - Confirm pending entries became settled, or keep them clearly marked as pending.
   - Remove duplicates from screenshots plus exports.
   - Replace generic ETF Investment with exact ticker/ISIN if available.
   - List uncategorized entries and missing-information items instead of hiding them.
4. Review cadence:
   - Monthly: light expense/investment summary.
   - Quarterly: allocation drift and ETF contribution pattern review.
   - Annually: broader strategy review, emergency cash, risk tolerance, taxes/records.

## Monthly summary format

Create or update a concise monthly summary using this structure:

```md
# Monthly Finance Summary — YYYY-MM

## Executive summary
- Total expenses: €X
- Total investments: €Y
- Investment rate / note: optional if income is known; otherwise omit.
- Overall note: 1-2 lines on whether spending/investing looked normal, high, or incomplete.

## Expenses by category
- Category A: €X
- Category B: €Y
- Uncategorized / needs review: €Z

## Expense entries worth noticing
- Date — Merchant/description — €Amount — Category — why notable

## Investments
- Total investments: €X
- Entries:
  - Date — Asset/ticker/ISIN if known — €Amount — Broker/account — Status/fees if known

## Notable changes vs previous month
- Spending increased/decreased in: category, if previous month exists.
- Investment contribution changed from €A to €B, if previous month exists.
- New recurring subscription, unusually large expense, or missing broker detail.

## Uncategorized or missing information
- Date — Description — €Amount — missing: category/account/status/ticker/ISIN/fees

## Next actions
- Confirm pending transactions.
- Clarify missing fields.
- Update ETF asset labels from generic to exact ticker/ISIN where possible.
```

## Monthly review checklist

At month end:
1. Add or import all bank/card/broker entries for the month.
2. Confirm pending entries became settled or remove duplicates.
3. Split entries only if one payment clearly contains multiple meaningful categories.
4. Review Expense total, Investment total, and monthly investment consistency.
5. For ETF contributions, replace generic ETF Investment with exact ticker/ISIN once known.
6. Produce the monthly summary and highlight uncategorized or missing-information items.

## Future-proofing rules

- Add new categories only when a recurring pattern appears; avoid over-categorizing one-off items.
- Keep Amount as a positive number; do not encode signs in the amount column.
- Keep Type limited to Expense or Investment unless a future redesign explicitly adds Income/Transfer.
- Keep raw source notes brief; do not store sensitive identifiers, card numbers, IBANs, or account numbers.
