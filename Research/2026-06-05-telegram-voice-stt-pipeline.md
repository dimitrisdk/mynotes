# Telegram Voice → STT → Agent Pipeline (Hermes Agent)

**Date:** 2026-06-05  
**Researcher:** Solomon (Σολομών)  
**Status:** Completed  
**Tags:** #hermes #telegram #stt #voice #transcription #pipeline

---

## Executive Summary

The Hermes Agent gateway **already has a complete, working pipeline** for Telegram voice message transcription. When a user sends a Telegram voice message (Opus/OGG), it is:
1. Downloaded and cached locally as `.ogg`
2. Classified as `MessageType.VOICE`
3. Routed through the STT pipeline via `_enrich_message_with_transcription`
4. Transcribed using the configured provider (default: `faster-whisper` local)
5. Prepended to the user's message as: `[The user sent a voice message~ Here's what they said: "transcript"]`

**Key distinction**: Telegram `msg.voice` (voice messages) → **always STT**; Telegram `msg.audio` (file attachments) → **never STT**, treated as file attachment.

---

## Complete Pipeline Flow

### 1. Telegram Adapter Reception (`gateway/platforms/telegram.py`)

```python
# Lines 5472-5481: Voice message handling
if msg.voice:
    file_obj = await msg.voice.get_file()
    audio_bytes = await file_obj.download_as_bytearray()
    cached_path = cache_audio_from_bytes(bytes(audio_bytes), ext=".ogg")
    event.media_urls = [cached_path]
    event.media_types = ["audio/ogg"]
```

- **Input**: Telegram `message.voice` (Opus in OGG container)
- **Cache**: `cache_audio_from_bytes()` → local file with `.ogg` extension
- **MIME**: `audio/ogg`
- **MessageType**: `MessageType.VOICE` (set by `_media_message_type()` at line 4945)

### 2. Message Classification (`telegram.py:4945-4957`)

```python
def _media_message_type(self, msg: Message) -> MessageType:
    if msg.voice:
        return MessageType.VOICE      # → STT pipeline
    elif msg.audio:
        return MessageType.AUDIO      # → file attachment, NO STT
    # ... other types
```

### 3. Gateway Message Processing (`gateway/run.py`)

**Entry point**: `handle_message` → `_handle_message` (line 7375) → `_prepare_inbound_message_text` (line 8500+)

**Media classification** (lines 8600-8615):
```python
if event.message_type == MessageType.AUDIO:
    audio_file_paths.append(path)          # Never STT
elif event.message_type == MessageType.VOICE or (
    mtype.startswith("audio/") 
    and event.message_type not in {MessageType.AUDIO, MessageType.DOCUMENT}
):
    audio_paths.append(path)               # Always STT
```

### 4. Transcription Enrichment (`run.py:15714-15806`)

```python
async def _enrich_message_with_transcription(self, user_text, audio_paths):
    if not getattr(self.config, "stt_enabled", True):
        # Return path + duration note, no transcription
    else:
        from tools.transcription_tools import transcribe_audio
        result = await asyncio.to_thread(transcribe_audio, path)
        if result["success"]:
            enriched_parts.append(
                f'[The user sent a voice message~ '
                f'Here\'s what they said: "{transcript}"]'
            )
```

### 5. Transcription Tool (`tools/transcription_tools.py:1615-1742`)

**Provider priority** (configurable via `stt.provider` in config.yaml):
1. **local** (default) — faster-whisper, free, auto-downloads model (~150MB for `base`)
2. **local_command** — custom CLI command via `HERMES_LOCAL_STT_COMMAND`
3. **groq** — free tier Whisper API, needs `GROQ_API_KEY`
4. **openai** — Whisper API, needs `VOICE_TOOLS_OPENAI_KEY` or `OPENAI_API_KEY`
5. **mistral** — Voxtral Transcribe, needs `MISTRAL_API_KEY`
6. **xai** — Grok STT, needs `XAI_API_KEY`
7. **elevenlabs** — Scribe, needs `ELEVENLABS_API_KEY`

**Local provider details** (`_transcribe_local`, lines 1110-1168):
- Singleton model cached in `_local_model` global
- Auto device selection: CUDA if available → CPU int8 fallback
- CUDA library load failures detected and retried on CPU
- Language: config `stt.local.language` > env `HERMES_LOCAL_STT_LANGUAGE` > auto-detect

---

## Configuration

### Gateway Config (`gateway/config.py:493`)
```python
stt_enabled: bool = True  # Auto-transcribe inbound voice messages
```

### STT Config (config.yaml)
```yaml
stt:
  enabled: true              # Global toggle
  provider: "local"          # local | local_command | groq | openai | mistral | xai | elevenlabs
  local:
    model: "base"            # tiny | base | small | medium | large-v3
    language: "en"           # Optional, forces language
  # Provider-specific sections:
  groq:
    model: "whisper-large-v3-turbo"
  openai:
    model: "whisper-1"
  mistral:
    model: "voxtral-mini-latest"
  xai:
    model: "grok-stt"
    format: true
    diarize: false
  elevenlabs:
    model_id: "scribe_v2"
```

---

## Failure Modes & Error Handling

| Scenario | Behavior |
|----------|----------|
| `stt_enabled: false` | Voice message noted with path + duration, no transcription |
| No STT provider configured | DM sent to user with setup instructions; transcript shows error note |
| Transcription error (network, API) | Error note prepended: `[The user sent a voice message but I had trouble transcribing it~ (error)]` |
| CUDA library missing | Auto-fallback to CPU int8, model reloaded |
| File > 25MB | Rejected with size error |
| Unsupported format | Rejected (supports mp3, mp4, mpeg, mpga, m4a, wav, webm, ogg, aac, flac) |

---

## Voice vs Audio File Distinction

| Telegram Field | MessageType | STT? | Treatment |
|----------------|-------------|------|-----------|
| `message.voice` | `VOICE` | ✅ Always | Transcribed, text prepended |
| `message.audio` | `AUDIO` | ❌ Never | File attachment note with path |
| `message.document` (audio mime) | `DOCUMENT` | ❌ Never | File attachment note with path |

**Tests confirming this** (`tests/gateway/test_telegram_audio_vs_voice.py`):
- `test_voice_message_still_transcribed` — VOICE → STT called
- `test_audio_attachment_skips_stt` — AUDIO → STT NOT called
- `test_audio_attachment_context_note_format` — AUDIO → file path note

---

## Key Files Reference

| File | Purpose |
|------|---------|
| `gateway/platforms/telegram.py:5472` | Voice caching (`.ogg`, `audio/ogg`) |
| `gateway/platforms/telegram.py:4945` | `_media_message_type` classification |
| `gateway/run.py:8600-8646` | Media routing: VOICE → `audio_paths` → STT |
| `gateway/run.py:15714` | `_enrich_message_with_transcription` |
| `tools/transcription_tools.py:1615` | `transcribe_audio()` main entry |
| `tools/transcription_tools.py:1110` | `_transcribe_local` (faster-whisper) |
| `gateway/config.py:493` | `stt_enabled` config |
| `agent/transcription_registry.py` | Provider registry for plugins |

---

## What This Means for Dimitris

1. **No setup needed** — If `faster-whisper` is installed, voice transcription works out of the box with the `local` provider (default).

2. **To enable voice transcription**:
   ```bash
   cd /usr/local/lib/hermes-agent
   uv pip install faster-whisper
   # or: pip install faster-whisper
   ```
   Then ensure `stt.enabled: true` in config.yaml (default).

3. **To use a cloud provider** (better accuracy):
   - Set `stt.provider: "groq"` + `GROQ_API_KEY` (free tier)
   - Or `stt.provider: "openai"` + `OPENAI_API_KEY` (paid)
   - Or others per the provider matrix above

4. **Audio file attachments (message.audio) are intentionally NOT transcribed** — this is by design to avoid transcribing music/podcasts sent as files.

5. **Failure visibility**: If STT fails, the user gets a DM with setup instructions. Check gateway logs for `[Telegram] Cached user voice at ...` and transcription errors.

---

## Related Research Notes

- [[Research/2026-06-05-hermes-audio-video-stt-tts-telegram]] — broader audio/video audit
- [[Research/2026-06-05-telegram-voice-stt-tts-link-summary-options]] — provider comparison
