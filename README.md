# Chatter Box

Compact, borderless, dark-neon offline AI chat GUI for Linux.
Built with **Rust + egui (eframe 0.29)**. Delivered as a single **AppImage**.
Runs on any modern Linux distro (tested on ParrotSec Plasma). No internet required at runtime.

> **Subtitle:** "Talk To AI In Any Language - Fully Offline"

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Screens](#screens)
4. [Requirements](#requirements)
5. [Installation](#installation)
6. [Usage Guide](#usage-guide)
7. [Multilingual Support](#multilingual-support)
8. [Media Generation](#media-generation)
9. [Chat Export / Import](#chat-export--import)
10. [Engine Management](#engine-management)
11. [Keyboard & Mouse](#keyboard--mouse)
12. [Build From Source](#build-from-source)
13. [AppImage Packaging](#appimage-packaging)
14. [Self-Test](#self-test)
15. [Project Layout](#project-layout)
16. [Configuration & Paths](#configuration--paths)
17. [Troubleshooting](#troubleshooting)
19. [License & Contact](#license--contact)

---

## Overview

Chatter Box is a lightweight, borderless desktop application that lets you chat with
**offline AI models** (text and graphics) running on your own machine through
[Ollama](https://ollama.com). It is designed for low-end hardware (tested on a
3 GB RAM laptop with Intel HD 520 + AMD Radeon, no CUDA) and ships as one
AppImage file.

- No cloud, no API keys, no telemetry.
- Detects Ollama engines automatically; if none are found, lets you browse to
  your AI folder.
- Text AI and Graphics AI (image / video generation) in one window.
- Multilingual: type in any language, the AI replies in the same language.
- Attach files (text or images) to a message.
- Copy, export and import chats as lossless `.txt`.

---

## Features

- Borderless window with a custom title bar:
  - **Top-left controls:** minimize (green), maximize/restore (blue), close (red),
    and a **Back** button (only on inner screens).
  - **Top-right:** **Contact** button (dark magenta) → opens the default browser
    to the GitHub repositories page.
- Red glowing title `Chatter Box`, green glowing outlined subtitle
  `Talk To AI In Any Language - Fully Offline`.
- Dark neon luminous theme with 3D metallic glowing buttons.
- WhatsApp-style chat bubbles (user right/blue, AI left/dark, avatars).
- Offline AI scanner: lists detected engines (text vs graphics).
- Models list with selectable chips; **Install models** (pull via Ollama or
  offline mode).
- Engine start / stop / rescan / browse folder.
- Media Studio for image and video generation.
- File picker (folder / file / import / save) with scrollable entries.
- Status bar + AI Engine console (color-coded log).
- AppImage deliverable with desktop entry and icons.

---

## Screens

### 1. Home / Scanner
- Title, subtitle, window controls.
- **Start AI Engine** / **Stop AI Engine** buttons.
- Engine status (auto-refreshed every 700 ms) + engine folder label.
- **Rescan**, **Browse** (folder picker).
- **Install models** row: text box + Install button + model chips.
- **Media Studio** button.
- Small AI Engine console (470 x 185).

### 2. Text Chat
- Left: chat bubbles, attach button, text input, send.
- Right sidebar: **Disconnect Chat**, console, **Import chat** (left) /
  **Export all chats** (right).
- Top of chat: model name, kind badge, **🌐 Any Language** toggle.

### 3. Graphics / Media Studio
- Prompt field, **Generate Image**, **Generate Video**, **Attach Reference**,
  **Open Media Folder**.
- Gallery with thumbnail previews and **Open** buttons.
- Console log.

---

## Requirements

Runtime:
- Linux x86_64 with a working X11/Wayland session.
- For text AI: [Ollama](https://ollama.com) 0.34+ (binary + at least one model).
- For media generation (optional): `ffmpeg`, `imagemagick` (`convert`).
- For opening files/links: `xdg-open` / `open` crate (bundled).

Build from source:
- Rust toolchain (stable) + Cargo.
- System libs: `libxkbcommon`, `libwayland`, `libx11`, `libgl`, fontconfig.
- Fonts: Noto Sans family (recommended, for multilingual rendering).

---

## Installation

### AppImage (recommended)

```bash
chmod +x Chatter_Box-x86_64.AppImage
./Chatter_Box-x86_64.AppImage
```

Optional: integrate into your app menu with
[AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher).

### From source

```bash
git clone https://github.com/RithiMangal/ChatterBox.git
cd ChatterBox
cargo build --release
./target/release/chatter_box
```

---

## Usage Guide

1. **Launch** the AppImage (or binary).
2. On the **Home** screen the app scans for Ollama engines.
   - If found, the engine folder and status appear.
   - If not found, click **Browse** and select the folder containing the
     `ollama` binary (e.g. `.../APPIMAGE/OLLAMA`).
3. Click **Start AI Engine**. Status changes to "AI engine running."
4. Models are fetched automatically. Click a model chip to select it, then
   **Connect & Chat**.
5. Type your message in the input box and press **Enter** or **Send**.
6. Use **📎 Attach** to attach a text file (inlined, up to 2000 chars) or an
   image (PNG/JPG, up to 12 MB) for vision-capable models.
7. **Disconnect Chat** unloads the model and returns to Home.
8. **Media Studio** (button on Home) opens the graphics screen for image/video
   generation.
9. **Contact** (top-right, magenta) opens the GitHub repositories page.

---

## Multilingual Support

- Type in **any language** (Hindi, Spanish, French, German, Chinese, Japanese,
  Arabic, Russian, etc.). The input box renders all scripts correctly thanks to
  the bundled Noto font set.
- The AI is instructed via a system prompt to **detect the language of your
  message and reply in that same language**, keeping grammar natural.
- Toggle **🌐 Any Language: ON/OFF** in the chat header to enable/disable the
  multilingual system prompt.
- Verified with `qwen2.5` family models (multilingual). Smaller models
  (`smollm2:360m`) may have limited language coverage.

---

## Media Generation

- **Generate Image**: prompt → PNG (768 x 768) via:
  1. A1111 (`127.0.0.1:7860`) if available,
  2. ComfyUI (`127.0.0.1:8188`) if available,
  3. Built-in offline renderer (ImageMagick plasma) as fallback.
- **Generate Video**: image → MP4 (zoompan + libx264) + poster PNG via `ffmpeg`.
- **Attach Reference**: attach an image as reference for image-to-image.
- **Open Media Folder**: opens `~/.cache/chatter_box/media/`.
- Gallery shows thumbnail previews; **Open** launches the file with the default app.

Output directory: `~/.cache/chatter_box/media/`

---

## Chat Export / Import

- **Copy chat**: copies the current conversation to the clipboard (xclip).
- **Export all chats**: saves all conversations to a `.txt` file (lossless
  format: `### Chatter Box chat export`, `=== CHAT n: name ===`, `[Sender]`).
- **Import chat**: loads a previously exported `.txt` back into a chat.
- Legacy `sender: text` format is also accepted on import.

---

## Engine Management

- Engine binary discovery order:
  1. Browsed AI folder (walkdir, depth 4, prefers `bin/`).
  2. Remembered path in `~/.config/chatter_box/engine_path`.
  3. Common paths (`~/Documents/APPIMAGE/OLLAMA/bin/ollama`, etc.).
  4. Running process executable.
  5. `PATH`.
- **Start AI Engine**: spawns `ollama serve`, logs to `/tmp/chatter_engine.log`.
- **Stop AI Engine**: SIGTERM → wait up to 3 s → SIGKILL.
- Status auto-polls every 700 ms (zombie-aware: `/proc/<pid>/stat`).
- Engine must be running before connecting a model.

---

## Keyboard & Mouse

- **Enter** in the input box sends the message.
- Window is borderless: move it with **Alt + drag** (Plasma) or by dragging the
  title area.
- Minimize / maximize / close via the top-left colored circles.

---

## Build From Source

```bash
# Debug build
cargo build

# Release build (LTO, panic=abort, stripped)
cargo build --release

# Run
./target/release/chatter_box

# Self-test (headless)
./target/release/chatter_box --selftest
./target/release/chatter_box --media-selftest
```

Dependencies (Cargo.toml):
`eframe 0.29`, `egui 0.29`, `serde`, `serde_json`, `dirs 5`, `open 5.1`,
`walkdir 2.4`, `image 0.25 (png)`.

---

## AppImage Packaging

```bash
chmod +x build_appimage.sh
./build_appimage.sh
# → Chatter_Box-x86_64.AppImage
```

The script builds the release binary, stages `AppDir/`, and invokes
`linuxdeploy` (downloads it if missing) to produce the AppImage.

Manual (offline):
```bash
linuxdeploy --appdir AppDir \
  --desktop-file AppDir/usr/share/applications/chatter_box.desktop \
  --icon-file AppDir/chatter_box.png \
  --output appimage
```

---

## Self-Test

`--selftest` verifies end-to-end:
- Ollama model list
- Chat reply ("Pong.")
- Model unload
- Vision payload rejection (text-only model)
- Export / import round trip
- Image generation (768 x 768 PNG)
- Video generation (~400 KB MP4)
- Base64 round trip

`--media-selftest` runs only the media pipeline.

---

## Project Layout

```
ChatterBox/
├── Cargo.toml
├── build_appimage.sh
├── assets/
│   ├── chatter_box.desktop
│   └── icons/hicolor/.../chatter_box.png
├── src/
│   ├── main.rs        # whole app (~2900 lines)
│   └── media.rs       # media backends (A1111, ComfyUI, local, ffmpeg)
├── Chatter_Box-x86_64.AppImage
└── README.md
```

---

## Configuration & Paths

| Path | Purpose |
|------|---------|
| `~/.config/chatter_box/engine_path` | Remembered Ollama binary path |
| `~/.cache/chatter_box/media/` | Generated images / videos / posters |
| `/tmp/chatter_engine.log` | Ollama serve log |
| `/tmp/chatter_run.log` | App stdout/stderr |

---

## Troubleshooting

- **"No engine/model selected."** → Start the AI Engine first, then select a
  model chip and Connect.
- **Model list empty** → Ollama not running or no models pulled.
  Run `ollama pull <model>` (e.g. `qwen2.5:1.5b`).
- **OOM / engine dies** → machine has only 3 GB RAM; use small models
  (`smollm2:360m`, `qwen2.5:0.5b`). Close other apps.
- **Boxes instead of letters** → install Noto fonts
  (`fonts-noto`, `fonts-noto-cjk`, `fonts-noto-color-emoji`).
- **Media fails** → install `ffmpeg` and `imagemagick`.
- **AppImage won't run** → `chmod +x`, and check FUSE
  (`apt install libfuse2` on Ubuntu 22.04+).
- **Window won't move** → use Alt+drag (Plasma) on borderless windows.


## License & Contact

- Contact / GitHub: https://github.com/RithiMangal?tab=repositories
  (opened only by the **Contact** button in the app.)
- (C) 2026. All rights reserved. License: TBD.

---

*Built with Rust. Runs offline. Talks any language.*
