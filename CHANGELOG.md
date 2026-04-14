# Changelog

All notable changes to PAM — Plant Automation Manager are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [2.2.0] — 2026-04-14

### Added
- **Transplant mode badge** — bed cards ≤7 days old show a pulsing `🌱 Day X transplant` badge; auto-removes on day 8
- **Sensors Only mode** — `📡 Sensors` toggle in header disables all AI calls with zero API cost

### Fixed
- **PAM summary markdown** — stripped `**bold**` and `*italic*` asterisks from AI output; added explicit no-markdown instruction to prompt
- **Bed modal markdown** — detail view also strips asterisks before rendering
- **Embed layout** — permanent compact layout for HA iframe cards; no `?mode=embed` param needed

### Changed
- All localStorage keys bumped to `pam22_*`
- Title updated to `PAM 2.2 — Plant Automation Manager`

---

## [2.0.0] — 2026-04-14

### Added

- **Sensors Only mode** — full dashboard with zero AI calls; toggled via 📡/🤖 button in header; state persists to localStorage
- **AI mode toggle button** — header button switches between `🤖 AI On` and `📡 Sensors` with visual state feedback
- **First-run API key modal** — on first load with no key, PAM auto-shows a setup modal; key saved to localStorage and never needs to be in the URL again
- **Key management** — 🔑 button in header opens modal to update or replace key at any time; validates `sk-ant-` prefix before saving
- **LLM-agnostic architecture** — `callClaude()` is a single swappable function; README includes drop-in code for OpenAI, Gemini, and local models (Ollama/LM Studio)
- **Auto-load AI after key entry** — saving a key in the modal triggers weather load and AI summary automatically if not already run
- **Guard rails** — all AI call sites check `AI_MODE` before firing; switching to Sensors Only mid-session cleanly disables all AI UI

### Changed

- API key no longer needs to be in the URL — localStorage replaces URL param as primary key storage
- `no-key-banner` now shows an "Enter Key →" button instead of just a text instruction
- All localStorage keys bumped from `pam10_*` to `pam20_*` to avoid conflicts with v1.0 cache
- README rewritten as LLM-agnostic — presents PAM as AI-provider-flexible with Claude as default

### Fixed

- `speakAdvice()` now correctly ignores the "AI disabled" message when Sensors Only mode is active

---

## [1.0.0] — 2026-04-14

### 🎉 Initial Public Release

First stable release of PAM. Evolved from an internal garden dashboard (garden_36.html)
into a clean, configurable, publicly shareable tool.

### Added

**Core Dashboard**
- Live WebSocket connection to Home Assistant — real-time sensor updates
- 3-bed layout with per-bed color coding (Tomato / Pepper / Strawberry)
- Sensor display per bed: soil temperature, moisture, light, conductivity, VPD
- Irrigation status strip: valve state, flow rate, leak detection, water supply
- Connection status badge with live/reconnecting indicator
- Clock display with day/date/time in header

**AI Features (Claude Haiku)**
- Per-bed ✨ Analyze button — health status, ideal ranges, 5-point actionable advice
- PAM summary panel — combined briefing across all beds, weather, and harvest status
- Shade schedule — AI-generated daily shade cloth timing per group, based on UV + forecast
- Direct, data-driven advice tone — no filler, numbers and actions only
- AI advice cache — persists last result to HA `input_text` helpers + `localStorage`
- Cache badges — shows freshness status (live vs cached) per panel
- 🔊 Speak button — sends PAM summary to any HA TTS media player

**Weather**
- Open-Meteo integration — free 4-day forecast, no API key required
- Local HA weather station fallback (ambient temp, humidity, UV, wind, rain)
- 4-day forecast strip with weather icons, high/low temps
- Feels-like temperature, rain today, wind speed pills

**Watered Tracking**
- 💧 Just Watered button — logs current timestamp to all 3 beds simultaneously
- Auto-detection — moisture rise ≥3% triggers automatic watered timestamp
- Debounce logic — 20-minute cooldown prevents duplicate stamps
- Persistent storage via HA `input_datetime` helpers (survives reboots)
- "Watered X ago" display per bed, updates every 60 seconds

**History Charts**
- 5-day moisture + temperature trend charts per bed via Chart.js
- Pulls from HA recorder history API
- Downsampled to 60 points for performance
- Loads automatically on WebSocket connect

**Harvest Countdown**
- Days-to-harvest countdown per bed with progress bar
- Planted date and days grown display
- Color states: waiting (white) → approaching (amber) → ready (green)
- Updates hourly

**Setup & Configuration**
- Single `CONFIG` block at top of script — all entity names, coordinates, bed definitions in one place
- Auto-built entity subscription list from CONFIG (no manual ENTITIES array maintenance)
- In-app YAML setup panel — shows required HA helpers with copy button, auto-hides when helpers exist
- Warning banner when Anthropic API key is missing from URL

**Security**
- API key and HA token passed via URL query parameters — nothing hardcoded in source
- No backend — all API calls go directly browser → Anthropic / HA

### Bed Configuration (Reference Setup)

| Bed | Contents | Sensor |
|-----|----------|--------|
| Bed 1 | Beefsteak Tomatoes + White Onions | Xiaomi/HHCC Bluetooth (via ESP32 proxy) |
| Bed 2 | Yolo Wonder Peppers + El Jefe Jalapeños + Onions | Xiaomi/HHCC Bluetooth (via ESP32 proxy) |
| Bed 3 | Allstar + Chandler Strawberries | Third Reality Zigbee (temp + moisture only) |

---

## Pre-release History (Internal)

> Not published to GitHub — documented here for reference.

| Version | Notable Changes |
|---------|----------------|
| garden_36.html | Final pre-release version — strawberry bed added, full 3-bed layout |
| garden_35.html | Shade schedule system added |
| garden_34.html | Cache system with HA input_text persistence |
| garden_33.html | History charts (Chart.js) added |
| garden_32.html | Harvest countdown added |
| garden_30.html | Watered auto-detection via moisture tracking |
| garden_28.html | Per-bed AI analysis with health strip |
| garden_25.html | TTS speak integration |
| garden_20.html | PAM (Pam) AI summary panel introduced |
| garden_15.html | WebSocket live connection to HA |
| garden_10.html | Initial prototype — static sensor display |

---

## Roadmap (Possible Future Additions)

- [ ] Configurable number of beds (not hardcoded to 3)
- [ ] Camera image analysis via ESPHome ESP32-S3 Sense
- [ ] PlantBook API integration for species-specific advice
- [ ] Fertilizer schedule tracking
- [ ] Mobile-optimized layout improvements
- [ ] Dark/light theme toggle
- [ ] Export history data to CSV

---

[1.0.0]: https://github.com/YOUR_USERNAME/pam/releases/tag/v1.0.0
