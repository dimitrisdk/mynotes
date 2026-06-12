# Telegram voice/STT/TTS/link-summary options

Date: 2026-06-05 07:47 UTC
Kanban task: `t_4f36a080`
Workspace artifact: `/root/.hermes/kanban/workspaces/t_4f36a080/telegram-voice-stt-tts-link-summary-options.md`

## Συμπέρασμα

- MVP: Telegram voice/audio -> existing media cache -> local faster-whisper STT -> normal text reply.
- Add Groq Whisper (`whisper-large-v3-turbo`) as first cloud fallback: official docs list $0.04/hour, 216x speed factor, OGG support, 99+ languages.
- Use Deepgram Nova-3 if production features matter: Greek `el` is supported; pricing examples are $0.29/hr monolingual and $0.35/hr multilingual.
- Keep TTS off by default. If used, generate OGG/Opus and mark it as Telegram voice; otherwise Telegram may send audio as a file/audio track.
- For links/videos: text-first. Firecrawl for web pages; YouTube transcript extraction first; audio STT only as fallback.

## Recommendation table

| Layer | MVP choice | Fallback | Notes |
|---|---|---|---|
| Telegram input | Existing Hermes Telegram media handling | Direct Bot API `getFile` debugging | Prior audit confirms voice/audio media is cached and STT is invoked when enabled. |
| STT | Local faster-whisper/Whisper, current `stt.provider=local` | Groq Whisper; Deepgram Nova-3 | Local = privacy/no per-minute cost. Groq = cheapest fast hosted fallback. Deepgram = richer production STT. |
| TTS | Off by default; text reply | Edge for low-cost personal use; ElevenLabs/Azure for API-backed quality | Avoid noisy UX and accidental cost. |
| Telegram voice output | OGG/Opus + `[[audio_as_voice]]` | MP3/M4A only if voice bubble not needed | Prior audit: Hermes Telegram routes native voice only for `.ogg`/`.opus` with voice flag. |
| YouTube summaries | `youtube-transcript-api` / captions first | `yt-dlp` subtitles, then audio download + STT | Official YouTube captions.download generally requires owner OAuth and costs 200 quota units. |
| General link summaries | Firecrawl scrape/search -> Markdown | browser/web_extract fallback | Firecrawl is already configured for Solomon. |

## Key facts

- OpenAI STT docs list `whisper-1`, `gpt-4o-mini-transcribe`, `gpt-4o-transcribe`, and diarization model; file upload limit 25 MB.
- Groq Speech-to-Text is OpenAI-compatible; `whisper-large-v3-turbo` supports OGG/WAV/WEBM/M4A/MP3, 99+ languages, and has minimum billed length 10 seconds.
- Deepgram Nova-3 docs list Greek (`el`); default language is `en`, so implementation must set language explicitly or use multilingual/auto mode.
- Google STT V2 standard starts at $0.016/min; dynamic batch is $0.003/min. Azure STT is $1/hr real-time and $0.18/hr batch, with 5 free hours/month.
- ElevenLabs supports Greek in Multilingual v2; Flash v2.5 targets low latency and lower character cost; pricing starts with free 10k credits/month and paid tiers.
- Firecrawl returns LLM-ready Markdown/JSON, handles JS rendering and PDF/doc parsing, and has a free tier/credit model.

## Assumptions / gaps

- Greek command accuracy on local Whisper `base` is not proven. Prototype with real Telegram clips is required.
- Latency claims are provider-side/docs, not measured from this VPS.
- Telegram official docs were reachable via search/browser snippets but not extractable by `web_extract`; live Bot API behavior should be verified.
- Privacy/data-retention terms need deeper review before sending sensitive audio to cloud STT/TTS.

## Εκτίμηση

Do not overbuild a real-time voice agent yet. Telegram voice notes are asynchronous, so the reliable first step is short-clip STT and text replies. Cloud fallback should be pluggable and measurable, not hardcoded. TTS should remain opt-in until Dimitris proves he wants spoken replies in daily use.

## Preferred MVP stack

1. Inbound Telegram voice/audio: cache original, collect metadata, normalize via `ffmpeg` when needed.
2. STT: local faster-whisper first; retry/fallback to Groq Whisper if empty/slow/low confidence; optional Deepgram Nova-3 for production mode.
3. Reply: text by default; include low-confidence transcript confirmation only when needed.
4. TTS: explicit user command or per-chat preference; produce OGG/Opus voice bubble.
5. Link summaries: Firecrawl for pages; YouTube transcript first; audio extraction + STT only after transcript failure and length/cost guard.

## Sources

- Telegram Bot API: https://core.telegram.org/bots/api
- python-telegram-bot Voice: https://docs.python-telegram-bot.org/en/v21.9/telegram.voice.html
- grammY file handling: https://grammy.dev/guide/files
- Prior Hermes audit: `/root/Documents/ObsidianVault/Research/2026-06-05-hermes-audio-video-stt-tts-telegram.md`
- OpenAI STT: https://developers.openai.com/api/docs/guides/speech-to-text
- Groq STT: https://console.groq.com/docs/speech-to-text
- Groq Whisper model: https://console.groq.com/docs/model/whisper-large-v3-turbo
- Deepgram languages/pricing: https://developers.deepgram.com/docs/models-languages-overview and https://deepgram.com/pricing
- Google STT pricing/languages: https://cloud.google.com/speech-to-text/pricing and https://docs.cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages
- Azure Speech: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support and https://azure.microsoft.com/en-us/pricing/details/speech/
- ElevenLabs TTS/pricing: https://elevenlabs.io/docs/overview/capabilities/text-to-speech and https://elevenlabs.io/pricing
- youtube-transcript-api: https://github.com/jdepoix/youtube-transcript-api
- YouTube captions.download: https://developers.google.com/youtube/v3/docs/captions/download
- Firecrawl: https://www.firecrawl.dev/ and https://docs.firecrawl.dev/

## Τι σημαίνει για τον Δημήτρη

Build the first version around “send a Telegram voice note, get a useful text answer.” That path uses existing Hermes plumbing and keeps privacy/cost under control. Add Groq fallback and YouTube/web text extraction before investing in always-on TTS or realtime voice-agent behavior.
