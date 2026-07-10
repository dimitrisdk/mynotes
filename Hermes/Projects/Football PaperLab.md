# Football PaperLab

## Σκοπός

Εργαλείο football analytics και **paper-only** tracking. Επιλέγει μόνο διαφανείς paper candidates για `1X2` και `Over/Under 2.5`.

- Δεν κάνει login σε bookmaker.
- Δεν τοποθετεί στοιχήματα.
- Δεν αποθηκεύει bookmaker credentials.
- Δεν παρακάμπτει VPN, γεωγραφικούς περιορισμούς ή όρους.
- Οποιαδήποτε νόμιμη live ενέργεια παραμένει χειροκίνητη από τον Δημήτρη.

## Dashboard

- **Football PaperLab:** [http://100.111.208.45:9130/](http://100.111.208.45:9130/)
- **Hermes HQ:** [http://100.111.208.45:9120/](http://100.111.208.45:9120/)
- **Polymarket PaperBot:** [http://100.111.208.45:8765/](http://100.111.208.45:8765/)

## Αρχικά rules

| Rule | Default |
|---|---|
| Markets | `1X2`, `OU25` |
| Minimum edge | 5 ποσοστιαίες μονάδες |
| Odds range | 1.50–4.50 |
| Minimum data quality | 0.75 |
| Paper bankroll | $1,000 |
| Per-trade cap | $25 |
| Aggregate exposure cap | $100 |

Απορρίπτει friendly, youth/U19, reserve αγώνες και διοργανώσεις εκτός της επιτρεπόμενης λίστας.

## Data source

Το MVP λειτουργεί τώρα με νόμιμο manual/public CSV import και demo fixtures. Δεν απαιτείται API key ή πληρωμή για το πρώτο στάδιο.

Για μελλοντικό πλήρως αυτοματοποιημένο feed, απαιτείται provider που επιτρέπεται να χρησιμοποιείται και, αν χρειάζεται, δικό του API key. Πριν από οποιαδήποτε χρέωση θα αξιολογηθεί free tier / όροι / coverage.

## Αρχεία και commands

- Project: `/root/football-paperlab`
- Database: `/root/football-paperlab/data/football-paperlab.sqlite3`
- Tests: `PYTHONPATH=src python3 -m unittest discover -s tests -v`
- Demo: `PYTHONPATH=src python3 run.py demo-import`

## Κατάσταση

- MVP παραδόθηκε και επαληθεύτηκε στις 2026-07-10.
- Suite: 5 tests passing.
- Χρειάζεται πραγματικό, επιτρεπόμενο CSV/data provider για παραγωγικά football candidates.
