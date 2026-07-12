---
title: Hermes Agent OS + Obsidian
created: 2026-06-22
updated: 2026-06-22
type: setup-note
tags: [hermes, obsidian, agent-os, memory, llm-wiki]
sources:
  - https://www.youtube.com/watch?v=ipYz0hFa8qA
  - https://youtube-transcript.ai/transcript/ipYz0hFa8qA.txt
confidence: medium
---

# Hermes Agent OS + Obsidian

## Τι κρατάμε από το video
- **Hermes** = το agent OS, δηλαδή το κέντρο που τρέχει τις δουλειές.
- **Obsidian** = η μόνιμη μνήμη / knowledge base σε απλά Markdown αρχεία.
- Το χρήσιμο setup δεν είναι ένα chat. Είναι **σύστημα**:
  - agents
  - notes
  - skills
  - workflows
  - guardrails

## Για τον Δημήτρη, το σωστό στήσιμο είναι
1. **Hermes στον VPS** ως κεντρικός agent.
2. **Obsidian vault στον VPS** ως κοινή μνήμη.
3. **Git sync** ώστε οι σημειώσεις να φτάνουν και στο PC.
4. **Skills** για επαναχρησιμοποιήσιμες διαδικασίες.
5. **LLM Wiki / knowledge base** για εξωτερική γνώση που μπορεί να διαβάζει ο Hermes.

## Προτεινόμενη δομή vault
- `Hermes/Memory.md` → μόνιμες σταθερές προτιμήσεις και facts.
- `Hermes/Operations/` → πρακτικά SOPs και συστήματα.
- `LLM Wiki/` → knowledge base για πηγές, concepts, raw imports.
- `LLM Wiki/raw/videos/` → transcripts / source dumps.
- `LLM Wiki/concepts/` → καθαρές έννοιες και συνδέσεις.

## Τι να μπει σαν κανόνας
- Μόνο ελληνικά στις απαντήσεις.
- Σύντομα, ξεκάθαρα βήματα.
- Πρώτα η απάντηση, μετά τα στοιχεία.
- Πηγές όταν υπάρχει εξωτερική πληροφορία.
- Keep the vault clean: raw source ξεχωριστά από τα distilled notes.

## Προτεινόμενο workflow
### 1. Ingest
- Παίρνουμε πηγή: video, άρθρο, PDF, meeting notes.
- Τη βάζουμε σε `raw/` note.

### 2. Distill
- Φτιάχνουμε ένα σύντομο concept note με:
  - τι είναι
  - γιατί με νοιάζει
  - τι σημαίνει για το setup μου

### 3. Connect
- Βάζουμε wikilinks μεταξύ:
  - `Hermes Memory`
  - `LLM Wiki`
  - related setup notes

### 4. Automate
- Αν μια πηγή επαναλαμβάνεται, τη βάζουμε σε cron / skill / workflow.

## Καλό πρακτικό takeaway
Το video ουσιαστικά λέει:
> *μην έχεις μόνο agent, να έχεις και μνήμη που μεγαλώνει*

Αυτό ταιριάζει πολύ στο setup του Δημήτρη:
- local-first
- VPS ως κέντρο
- Obsidian ως source of truth
- Hermes ως executor

## Επόμενο βήμα
Να φτιάξουμε:
- ένα **LLM Wiki concept note** για αυτό το setup
- ένα **SOP** για το πώς περνάμε νέες πηγές από video → raw note → distilled note → skill

## Session Archive / Second Brain (2026-07-11)

### Τι αποθηκεύεται
- Ο exporter διαβάζει **read-only** το default Hermes session database (`/root/.hermes/state.db`).
- Κρατά μόνο μηνύματα `user` και `assistant`, οργανωμένα ανά συνεδρία και ημερομηνία Europe/Athens.
- Τα daily logs είναι στο `Hermes/Chat Logs/YYYY-MM-DD.md` και δεν έχουν αυτόματο retention/delete policy: διατηρούνται μέχρι ρητή ανθρώπινη απόφαση.
- Κάθε block έχει ασφαλές σύντομο session ID, source, deterministic summary, decisions, completed/open actions, links και transcript.

### Τι δεν αποθηκεύεται
- Raw tool output, tool calls, system prompts και reasoning fields.
- API keys, passwords, bearer/OAuth/bot tokens, cookies, private keys και credential-file content. Secret-like values redacted πριν από κάθε write.
- Το `Hermes/Memory.md` δεν ενημερώνεται αυτόματα: εκεί μπαίνουν μόνο ρητά, ασφαλή durable facts.

### Λειτουργία και ασφάλεια
1. `/root/.hermes/scripts/export_sessions_to_obsidian.py` τρέχει ανά 15 λεπτά ως script-only cron, χωρίς LLM.
2. Κρατά atomic watermark στο `/root/.hermes/state/obsidian-session-export.json` και lock για overlap protection.
3. Κάθε transcript message έχει hidden ID marker. Αν το state χαλάσει μετά από write, ο exporter ανακτά τη γνώση από τα markers και δεν διπλασιάζει μηνύματα.
4. Success είναι σιωπηλό. Failure επιστρέφει μόνο σύντομο diagnostic στον scheduler.
5. Ο υπάρχων daily `🗂️ Obsidian Sync — Daily` παραμένει ο μόνος Git push workflow.

### Safe rebuild / verification
- Dry-run (δεν γράφει τίποτα):
  `python3 /root/.hermes/scripts/export_sessions_to_obsidian.py --dry-run`
- Tests με isolated SQLite/vault fixtures:
  `python3 -m unittest discover -s /root/.hermes/tests -v`
- Για ασφαλές rebuild, κράτα backup του state file και χρησιμοποίησε νέο temporary vault. Μην σβήνεις υπάρχοντα logs: τα hidden markers είναι μέρος της deduplication/recovery ασφάλειας.
