# IT Operations Log
This file tracks all technical changes, installations, and maintenance performed by Petros.

| Date | Task | Status | Notes |
| :--- | :--- | :--- | :--- |
| 2026-05-30 | Initial IT log setup | Completed | Petros activated |
| 2026-05-30 | Install Home Assistant (Docker) | Completed | Home Assistant running on host network, data in /root/homeassistant/config |
| 2026-06-02 | Telegram bots για Petros & Samson | Completed | Petros: @aipetros_bot, Samson: @aisasmson_bot — .env & config.yaml updated |
| 2026-06-02 | Systemd services για gateways | Completed | hermes-gateway-petros.service & hermes-gateway-samson.service — enabled on boot, auto-restart |
| 2026-06-02 | Πρωινό cron TODO tasks | Completed | Κάθε μέρα 09:00 Ελλάδας (06:00 UTC) — στέλνει τα TODO tasks από το vault |
| 2026-06-02 | Πρωινό news brief | Completed | Κάθε μέρα 10:00 Ελλάδας (07:00 UTC) — στέλνει Greece/tech/AI/Hermes news με links |
| 2026-06-02 09:48 UTC | Model update στα Hermes profiles | Completed | Default, Petros, Ioudas, Samson σε model.default=gpt-5.5 και provider=openai-codex. OpenAI Codex auth διαθέσιμο σε όλα, smoke tests OK, gateways Panagia/Petros/Samson restarted. Default profile πήρε model.context_length=65536. |
| 2026-06-02 10:12 UTC | Panagia model alignment | Completed | `model.default` στο Panagia ορίστηκε σε gpt-5.5 (provider: openai-codex) ώστε να ταιριάζει με τα υπόλοιπα profiles. |
| 2026-06-02 19:04 UTC | Restic local backup system | Completed | UFW checked first: inactive. Installed restic, initialized encrypted local repo at `/root/backups/restic`, created `/root/backup-scripts/restic-vps-backup.sh`, enabled `restic-vps-backup.timer` daily at ~03:30 UTC. First backup snapshot `c36ead4c` completed: HA config, Obsidian vault, Hermes profiles, Ollama volume metadata excluding model blobs; `restic check` passed; repo size 51M. No ports opened. |
| 2026-06-02 19:29 UTC | Ioudas profile/goal update | Completed | Updated `/root/.hermes/profiles/ioudas/SOUL.md`: Greek-first personal Financial Advisor profile focused on ETF portfolio oversight, risk, fees, allocation, rebalancing, and Greek/EU investor context. Updated Petros team workflow skill to reflect Ioudas role. Smoke test with `hermes --profile ioudas chat` returned Greek response. |
| 2026-06-02 19:53 UTC | Ioudas Telegram gateway | Completed | Added Telegram bot token to `/root/.hermes/profiles/ioudas/.env`, restricted access/home to Dimitris chat/user `6060749286`, set `telegram.allowed_chats=6060749286`, created/enabled/started `hermes-gateway-ioudas.service`. Bot API `getMe` OK (`judasai_bot`), systemd active. Direct send test returned `chat not found`, so Dimitris must first open/start the bot in Telegram. |
