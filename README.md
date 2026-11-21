# 📹 Meeting Recording Tool (會議錄製工具)

A fully browser-based meeting recording toolkit for screen capture, video recording, and audio extraction — no server, no installation, no plugins required. The entire UI is presented in **Traditional Chinese (繁體中文)**.

## 🌟 Overview

This single-page HTML application leverages modern Web APIs (`MediaRecorder`, `getDisplayMedia`, `Web Audio API`, `OfflineAudioContext`) to provide a professional-grade recording and audio extraction workflow entirely within the browser.

---

## ✨ Features

### 🎬 Screen Recording

- **Format auto-detection** — Automatically detects browser-supported formats and offers available options (WebM VP9/VP8, MP4 H.264). Displays a notice when MP4 is unavailable.
- **Quality presets** — High (8 Mbps), Medium (4 Mbps), and Low (2 Mbps) bitrate options.
- **System audio capture** — Record audio from the shared screen, tab, or application (browser-permitting).
- **Microphone input** — Optionally mix microphone audio into the recording alongside system audio.
- **Pause / Resume** — Pause and resume recording at any time without splitting into separate files.
- **Live microphone mute/unmute** — Toggle microphone on or off mid-recording with a smooth gain ramp (no audio pop). The UI reflects the current mute state with a color-coded button and indicator badge.
- **Recording timer** — Displays elapsed recording time in `HH:MM:SS` format with a pulsing red dot indicator. Correctly accounts for paused durations.

### 📷 Screenshot Capture

- One-click screen capture via `getDisplayMedia`.
- Saves as PNG with automatic timestamped filenames.

### ⏰ Scheduled Recording

- **Absolute time mode** — Set a specific date and time for the recording to start automatically. Date and time inputs are pre-filled with a sensible near-future default.
- **Relative delay mode** — Set a countdown delay in hours, minutes, and seconds. Quick-select preset buttons are provided for common intervals: 1 min, 5 min, 10 min, 15 min, 30 min, and 1 hour.
- **Live countdown banner** — A prominent gradient banner displays the target time, a real-time countdown (`HH:MM:SS`), and an animated progress bar.
- **Audio cues** — Audible beep tones play during the final 5-second countdown and at the moment recording begins.
- **Cancel support** — The scheduled recording can be cancelled at any time before it triggers.
- **Auto-stop timer** — Optionally configure a maximum recording duration. The recording stops automatically after the specified time, with a dual-beep audio alert. The auto-stop target is displayed alongside the recording timer.

### 🔊 Real-Time Audio Level Monitoring

- **Per-source meters** — Independent level meters for System audio, Microphone, and Mixed output (shown only when both sources are active).
- **RMS dB calculation** — Accurate real-time RMS levels displayed in decibels with color-coded labels (green for normal, yellow for moderate, red for clipping).
- **Peak hold indicators** — White peak markers with configurable hold time (1.5 s) and smooth decay.
- **Graduated scale** — Tick marks at -60, -48, -36, -24, -12, -6, and 0 dB for easy reference.
- **Gradient meter fills** — Each meter uses a gradient (green → yellow → red) to visually indicate level severity.
- **Status badge** — A dynamic badge reports overall audio health: ✓ Good Signal, ⚠ Low Signal, ⚠ Very Low, 🔴 Clipping, ⏸ Silent, ✖ No Signal (with blinking animation), or … Waiting.
- **Contextual warning messages** — Inline troubleshooting tips appear when no system audio or microphone signal is detected, when the mic is muted, or when clipping is occurring.

### 🎵 Audio Extraction

- **Two source modes:**
  - **Session recordings** — Extract audio from any video recorded in the current session via a dropdown selector.
  - **File upload** — Upload an external video/audio file via file picker or drag-and-drop. Supports WebM, MP4, OGG, and MKV.
- **Output formats:**
  - **WAV** — Uncompressed PCM 16-bit.
  - **MP3** — Encoded via `lamejs` with selectable bitrate (64 / 128 / 192 / 320 kbps).
  - **WebM Audio** — Opus codec via `MediaRecorder`.
- **Sample rate selection** — 48 kHz, 44.1 kHz, 22.05 kHz, or 16 kHz. Automatic resampling is performed via `OfflineAudioContext` when the source rate differs from the target.
- **Fallback decoding** — If the primary `decodeAudioData` path fails, a secondary approach using `MediaElementSource` and real-time re-recording is attempted.
- **Progress indicator** — A progress bar and status text track each phase: reading, decoding, resampling, encoding, and waveform generation.
- **Waveform visualization** — A canvas-rendered waveform of the extracted audio is displayed with a dark background and purple amplitude trace.
- **Inline playback** — An HTML5 audio player is provided for immediate preview of the extracted audio.
- **Quick extract** — A one-click "🎵 Audio" button on each video card in the capture gallery triggers extraction with default settings.

### 📁 Capture Gallery

- **Grid layout** — All captures (screenshots and videos) are displayed as cards with visual thumbnails.
- **Metadata display** — Each card shows the filename, format badge (PNG / WEBM / MP4), and file size.
- **Per-item actions** — Download, delete, or quick-extract audio for each capture.
- **Bulk actions** — "Download All" and "Clear All" buttons for batch operations.
- **Capture counter** — The section header displays the total number of captures.

### 🎧 Extraction History

- Maintains a log of all extracted audio files within the session.
- Each entry displays: filename, format badge, channel count, sample rate, duration, and file size.
- Inline audio player and individual download/delete buttons for each entry.

---

## 🛠 Technology

| Layer | Technology |
|-------|------------|
| UI Framework | Vanilla HTML / CSS / JavaScript |
| Styling | Tailwind CSS (CDN) |
| Screen Capture | `navigator.mediaDevices.getDisplayMedia()` |
| Microphone | `navigator.mediaDevices.getUserMedia()` |
| Recording | `MediaRecorder` API |
| Audio Mixing | `Web Audio API` (`AudioContext`, `MediaStreamDestination`, `GainNode`) |
| Level Metering | `AnalyserNode` with `getFloatTimeDomainData()` |
| Audio Decoding | `AudioContext.decodeAudioData()` / `OfflineAudioContext` |
| Resampling | `OfflineAudioContext` |
| WAV Encoding | Custom PCM encoder (JavaScript) |
| MP3 Encoding | [lamejs](https://github.com/zhuker/lamejs) (CDN) |
| WebM Audio Encoding | `MediaRecorder` with Opus codec |
| Scheduled Beeps | `OscillatorNode` + `GainNode` (Web Audio API) |

---

## 🌐 Browser Compatibility

| Feature | Chrome / Edge | Firefox | Safari |
|---------|:---:|:---:|:---:|
| Screen Recording | ✅ | ✅ | ⚠️ Limited |
| System Audio Capture | ✅ | ❌ | ❌ |
| Microphone Mixing | ✅ | ✅ | ✅ |
| WebM Output | ✅ | ✅ | ❌ |
| MP4 Output | ✅ (v125+) | ❌ | ✅ |
| MP3 Encoding (lamejs) | ✅ | ✅ | ✅ |
| Scheduled Recording | ✅ | ✅ | ✅ |

> **Recommended:** Google Chrome or Microsoft Edge (latest) for full feature support including system audio capture and MP4 output.

---

## 🚀 Getting Started

1. Open the HTML file in a supported browser.
2. Configure recording settings (format, quality, audio sources).
3. *(Optional)* Enable scheduled recording and set a start time or delay.
4. *(Optional)* Enable auto-stop and set a maximum duration.
5. Click **⏺ 開始錄製** (Start Recording) or **⏰ 排程錄製** (Schedule Recording).
6. Grant screen sharing and microphone permissions when prompted.
7. Use **⏸ 暫停** / **▶ 繼續** to pause/resume, and **🎙 / 🔇** to mute/unmute the mic.
8. Click **⏹ 停止** (Stop) to end the recording.
9. Use the **🎵 音訊提取** (Audio Extraction) section to extract audio tracks.
10. Download captures and extracted audio from the gallery and history sections.

---

## 📂 File Structure

```
meeting-recording-tool.html   # Single self-contained HTML file (no build step required)
README.md                     # This file
```

---

## ⚠️ Limitations

- **System audio** is only available in Chromium-based browsers and requires the user to explicitly share a tab or screen with audio enabled.
- **MP4 recording** requires Chrome/Edge 125+ or Safari. A notice is shown when unavailable.
- All data is held **in-memory** within the browser session. Refreshing or closing the page will discard all captures and extractions. Download important files promptly.
- `localStorage` and `sessionStorage` are not used; state does not persist across sessions.
- Very long recordings may consume significant memory depending on bitrate and duration.
- MP3 encoding via `lamejs` is performed synchronously on the main thread and may briefly block the UI for very large files.

---

## 📝 License

This project is provided as-is for personal and educational use.
