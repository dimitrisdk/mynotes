# Hermes-Agent audio/video handling audit

Date: 2026-06-05 01:43 UTC
Kanban task: `t_5ba47002`
Workspace artifact: `/root/.hermes/kanban/workspaces/t_5ba47002/stt-tts-telegram-audio-video-audit.md`

## Συμπέρασμα

- STT lives mainly in `/usr/local/lib/hermes-agent/tools/transcription_tools.py`; gateway logic decides when to call `transcribe_audio()`.
- TTS lives mainly in `/usr/local/lib/hermes-agent/tools/tts_tool.py`; `text_to_speech_tool()` creates audio and emits `MEDIA:<path>` plus usually `[[audio_as_voice]]`.
- Current config has `stt.enabled: true`, `stt.provider: local`, local model `base`, `tts.provider: edge`, and `voice.auto_tts: false`.
- Telegram has strict routing: `.ogg`/`.opus` become native voice bubbles only when flagged as voice; `.mp3`/`.m4a` are sent as audio; `.wav`/`.flac` fall back to documents.
- Video attachments are handled as media/files, not directly transcribed by the STT flow.

## Key source map

- `/usr/local/lib/hermes-agent/gateway/platforms/telegram.py`
  - `_handle_media_message`: downloads/caches Telegram media and normalizes events.
  - `send_voice()` lines ~3709-3798: `.ogg`/`.opus` -> Bot API `send_voice`; `.mp3`/`.m4a` -> `send_audio`; other audio -> document.
- `/usr/local/lib/hermes-agent/gateway/platforms/base.py`
  - `should_send_media_as_audio()` lines ~102-122: platform-specific audio routing, special Telegram rules.
  - `cache_media_bytes()` lines ~1313-1366: classifies bytes into image/video/audio/document.
  - `extract_media()` lines ~2860-2941: extracts `MEDIA:<path>` and `[[audio_as_voice]]` from agent responses.
- `/usr/local/lib/hermes-agent/gateway/run.py`
  - STT orchestration in the message loop.
  - post-response media delivery around lines ~12240-12348.
  - appends tool-result media tags to final responses around lines ~18164-18198.
- `/usr/local/lib/hermes-agent/tools/transcription_tools.py`
  - `transcribe_audio()` and providers: local/faster-whisper, OpenAI, Mistral, Groq, ElevenLabs.
- `/usr/local/lib/hermes-agent/tools/tts_tool.py`
  - `text_to_speech_tool()` and providers: Edge, ElevenLabs, OpenAI, MiniMax, Mistral/Voxtral, Gemini, xAI, NeuTTS, KittenTTS, Piper.

## Flow: Telegram voice input -> STT

1. Telegram adapter receives a voice/audio update.
2. Media is downloaded and cached as an agent-visible local file.
3. Gateway sees audio/voice media and checks `stt.enabled`.
4. Gateway calls `transcribe_audio()` with configured provider (`local` currently).
5. Transcript text is fed into the normal conversation path.

## Flow: TTS output -> Telegram voice/audio

1. Agent/tool calls `text_to_speech_tool()`.
2. Tool writes an audio file and returns delivery tags, e.g. `[[audio_as_voice]]` and `MEDIA:/path/file.ogg`.
3. Gateway extracts the media tag with `BasePlatformAdapter.extract_media()`.
4. Gateway calls `should_send_media_as_audio()` to decide audio-vs-document route.
5. Telegram adapter sends:
   - `.ogg`/`.opus` + voice flag -> `send_voice()` voice bubble.
   - `.mp3`/`.m4a` -> `send_audio()`.
   - other formats -> `send_document()`.

## Gaps

- No live Telegram round-trip was executed.
- Provider runtime success still depends on credentials, network, local models, dependency availability, and `ffmpeg`.
- No evidence that video files are automatically demuxed and transcribed; that would require explicit audio extraction before STT.

## Τι σημαίνει για τον Δημήτρη

For the current setup, voice input should use local Whisper/faster-whisper STT, while voice output defaults to Edge TTS. The fragile point for Telegram UX is not provider selection but file format/directive routing: native voice bubbles require `.ogg`/`.opus` plus `[[audio_as_voice]]`.
