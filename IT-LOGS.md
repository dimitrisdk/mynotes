# IT Operations Log
This file tracks all technical changes, installations, and maintenance performed by Petros.

| Date | Task | Status | Notes |
| :--- | :--- | :--- | :--- |
| 2026-05-30 | Initial IT log setup | Completed | Petros activated |
| 2026-05-30 | Install Home Assistant (Docker) | Completed | Home Assistant running on host network, data in /root/homeassistant/config |
| 2026-06-02 | Telegram bots για Petros & Samson | Completed | Petros: @aipetros_bot, Samson: @aisasmson_bot — .env & config.yaml updated |
| 2026-06-02 | Systemd services για gateways | Completed | hermes-gateway-petros.service & hermes-gateway-samson.service — enabled on boot, auto-restart |
| 2026-06-02 | Πρωινό cron TODO tasks | Completed | Κάθε μέρα 09:00 Ελλάδας (06:00 UTC) — στέλνει τα TODO tasks από το vault |
| 2026-06-02 09:48 UTC | Model update σε όλα τα Hermes profiles | Completed | Default, Panagia, Petros, Ioudas, Samson σε model.default=gpt-5.5 και provider=openai-codex. OpenAI Codex auth διαθέσιμο σε όλα, smoke tests OK, gateways Panagia/Petros/Samson restarted. Default profile πήρε model.context_length=65536. |
