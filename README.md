# ⚡ FocusFlow

**Study · Focus · Compete**

A local-first study workspace in the browser. FocusFlow combines a focus timer, on-device AI
attention tracking, task planning, quizzes, coding practice, group study rooms and ambient sound
in one distraction-free interface — with no accounts, no backend and no install.

![Vanilla JavaScript](https://img.shields.io/badge/JavaScript-vanilla-F7DF1E?logo=javascript&logoColor=black)
![No dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)
![No backend](https://img.shields.io/badge/backend-none-blue)
![Offline capable](https://img.shields.io/badge/offline-capable-informational)

---

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [By the Numbers](#by-the-numbers)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Optional Configuration](#optional-configuration)
- [Privacy & Data](#privacy--data)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Browser Support](#browser-support)
- [Known Limitations](#known-limitations)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Acknowledgements](#acknowledgements)

---

## Introduction

Students juggle a timer in one tab, a to-do list in another, a quiz site, a code editor and a
video call — and lose focus switching between them. **FocusFlow** brings all of it into a single
page that runs entirely on the student's own device.

At its core is a focus timer paired with an **AI Focus Guard**: using MediaPipe FaceMesh inside the
browser, it watches the webcam and gives one clear signal — green when you are facing the screen,
red when you look away, close your eyes or pick up your phone. Around that core sit a task planner,
timed quizzes across six subjects, a sandboxed coding playground and a "Tape Deck" of generated
soundscapes and curated Hindi music.

Written in plain HTML, CSS and vanilla JavaScript, FocusFlow has no build step, no package manager
and no server. Every session, score and note is stored locally in the browser.

> **Project type:** academic / self-initiated web application project
> **Status:** actively developed · single-page static app

---

## Features

### 🎯 Focus & Attention
- **Three ways to study** — open-ended stopwatch, configurable Pomodoro (25/5, 50/10, 90/20 or custom), or guard-monitored sessions.
- **AI Focus Guard** — on-device face and eye tracking through MediaPipe FaceMesh; flags looking away, closed eyes and absence. Optional phone detection via COCO-SSD. Three sensitivity levels.
- **Minimal mode** — strips a live session down to the timer ring and Pause/Finish.
- **Session summary** — focus percentage, XP earned, streak update and distraction log.

### 📊 Progress & Motivation
- **XP and levels** — 1 XP per focused minute; XP never accrues from quizzes or code, so study time stays the primary metric.
- **Streaks and badges** — daily streak tracking with unlockable achievement badges.
- **Dashboard** — weekly study chart, monthly calendar, subject breakdown and recent sessions.
- **Profile** — cumulative totals, best sessions, level progress and history.

### ✅ Planning
- Task list with subjects, priority levels, due dates, completion state and automatic sorting.

### 🎓 Practice
- **Quizzes** — 120 built-in questions across Biology, English, Computer Science, History, Geography and Economics, with explanations and instant scoring.
- **Code practice** — write and run solutions in-browser:
  - **JavaScript** and **Python** execute inside a sandboxed Web Worker (JavaScript is fully local; Python uses Pyodide loaded from a CDN on first use).
  - **C, C++, Java** are available only through an optional remote runner that is **off by default** and requires explicit consent every session.

### 👥 Study Rooms
- Create or join a room by code, pick a subject and a session length, and study alongside ranked peers. *(Currently a local simulation — see [Known Limitations](#known-limitations).)*

### 🎧 Tape Deck
- **19 generated sound channels** — instrumental (jazz, lofi, piano, synthwave, bossa, strings, baroque, night drive, singing bowls, music box), noise (white, brown, pink) and nature (rain, thunderstorm, café, fireplace, ocean, night forest). All synthesised live with the Web Audio API — no audio files.
- **7 curated Hindi music stations** streamed through the official YouTube IFrame API, opt-in and revocable.
- **My Files** — load your own local tracks, stored in IndexedDB and never uploaded.

### 🎨 Personalisation
- Six themes — **Dark, Light, Aurora, Sakura, Matcha, Mocha** — plus optional background art.
- Custom subjects, adjustable volumes, music shuffle/repeat, reduced-motion support and a fully responsive layout.

---

## By the Numbers

| | |
|---|---|
| Quiz subjects | 6 (120 questions) |
| Programming languages | 5 |
| Sound channels | 19 (all generated) |
| Music stations | 7 (curated, opt-in) |
| Themes | 6 |
| npm dependencies | 0 |
| Backend services | 0 |

---

## Tech Stack

| Layer | Choice | Why |
|---|---|---|
| Markup & styling | Semantic HTML5, modern CSS (custom properties, grid, flex) | Zero build step, instant load |
| Logic | Vanilla JavaScript (classic scripts, IIFE modules) | Runs from any static host |
| Persistence | `localStorage` via a single `Store` wrapper; `IndexedDB` for audio files | Fully local, no server |
| Attention tracking | MediaPipe FaceMesh + TensorFlow.js COCO-SSD | On-device inference, frames never uploaded |
| Code execution | Web Workers from Blobs with hard timeouts | Sandboxed, no `eval` on the main page |
| Python runtime | Pyodide (CDN, lazy-loaded) | Real Python in the browser |
| Audio | Web Audio API (synthesis) | No audio assets shipping |
| Music | YouTube IFrame Player API | Licensed playback, no extraction |
| Icons & fonts | Font Awesome, Space Grotesk, JetBrains Mono (CDN) | Graceful system-font fallback offline |

---

## Project Structure

```
MP MAIN/
├── index.html               # App shell: login, chrome, all views, modals
├── AGENTS.md                # Architecture and contribution rules
├── assets/                  # Optimised WebP background art (one per theme)
├── css/
│   ├── base.css             # Tokens, reset, chrome, themes
│   └── views.css            # Per-view styles
└── js/
    ├── core.js              # Helpers, Store, toast, themes, subjects
    ├── audio.js             # Tape Deck synthesis engine
    ├── music.js             # YouTube stations + local files
    ├── guard.js             # AI Focus Guard (FaceMesh / COCO-SSD)
    ├── tracker.js           # Session engine, XP, badges, focus UI
    ├── tasks.js             # Task planner
    ├── quiz.js              # Quiz engine
    ├── quizdata*.js         # Question banks (6 subjects)
    ├── runners.js           # Language adapters & sandboxed execution
    ├── code.js              # Code practice UI
    ├── rooms.js             # Study rooms (simulated peers)
    ├── profile.js           # Profile and history
    ├── dashboard.js         # Home view analytics
    ├── tips.js              # Contextual study tips
    ├── demo.js              # Demo data load / restore
    └── app.js               # Login, hash router, boot (loads last)
```

> **Note:** scripts are classic (non-module) and share a single global namespace. **Load order in
> `index.html` matters** — `app.js` must stay last. Keep new files as classic scripts.

---

## Getting Started

### Requirements
- A modern browser (Chrome, Edge, Firefox or Safari)
- No Node.js, npm or build tools

### Run locally

```bash
git clone <your-repository-url>
cd "MP MAIN"
```

Then serve the folder with any static server — for example:

```bash
python3 -m http.server 5500
# then open http://localhost:5500
```

Or use the **Live Server** extension in VS Code and open the generated `http://127.0.0.1:5500` URL.

> **Important:** opening `index.html` directly via `file://` works for most features, but the
> **AI Focus Guard** and the **Tape Deck music player** require a real origin (`localhost` or
> HTTPS). Use a local server for the full experience.

On first launch, enter a display name and pick a colour — or use **Quick demo login** to explore
with sample data. `Demo.restore()` in the browser console puts real data back afterwards.

---

## Optional Configuration

These are opt-in and disabled by default:

| Feature | How to enable |
|---|---|
| **YouTube music** | Accept the consent modal in the Tape Deck → Music tab (`ff_ytConsent = 'yes'`). Revocable at any time. |
| **Remote code runner** (C, C++, Java) | Set the localStorage key `ff_runnerurl` to your runner endpoint. It receives `POST` JSON `{ language, code, stdin }` and must return stdout/stderr. Consent is requested per session. |
| **AI Focus Guard** | Toggle it on in the Focus view and allow camera access. |
| **Custom background art** | Theme popover → Background art → On/Off. |

> The bundled remote-runner client expects a JSON runner service you host yourself; it is a
> deliberate placeholder and ships disabled.

---

## Privacy & Data

FocusFlow is built so that **nothing leaves the device by default**.

- **Camera** — opt-in only, processed locally, stopped when the Guard is off. No frames are stored or transmitted.
- **Storage** — all app state lives in `localStorage` under the `ff_` prefix, plus IndexedDB
  database `ff_tape_v1` for local audio files.
- **Network** — only three outbound calls exist, and all are either content delivery or opt-in:
  1. CDN assets (fonts, icons, and the FaceMesh/Pyodide libraries).
  2. YouTube playback, only after explicit consent.
  3. A remote code runner, only if you configure one and consent per session.
- **No analytics, no accounts, no cookies, no tracking.**

To wipe everything, clear site data for the origin in your browser settings.

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `1` – `6` | Switch views (Home, Focus, Tasks, Practice, Rooms, Tape Deck) |
| `Esc` | Close any open modal or popover |

---

## Browser Support

| Browser | Support |
|---|---|
| Chrome / Edge (latest) | Full |
| Firefox (latest) | Full |
| Safari 16+ | Full (camera and Web Audio supported) |
| Mobile browsers | Responsive UI; camera-based Guard works best on desktop |

`prefers-reduced-motion` is respected throughout: decorative animation and transitions are disabled.

---

## Known Limitations

- **Study rooms are a local simulation.** Peers and rankings are generated locally; there is no
  real-time backend yet.
- **The remote code runner is a placeholder.** C, C++ and Java require you to point
  `ff_runnerurl` at your own service.
- **Python needs a one-time CDN download** (Pyodide). JavaScript runs fully offline.
- **CDN typography and icons** degrade to system fallbacks when offline.
- **YouTube embeds may show their own ads.** This is why streaming is opt-in and clearly labelled.

---

## Roadmap

- [ ] Optional real-time backend for study rooms
- [ ] Verified remote runner service for C, C++, Java
- [ ] Spaced-repetition scheduling for quizzes
- [ ] Exportable study reports (CSV / PDF)
- [ ] Installable PWA with full offline asset caching
- [ ] Teacher / classroom dashboards

---

## Contributing

Contributions are welcome. Because the app is a no-build static site, please keep it that way:

1. **No new tooling.** No bundlers, frameworks, npm dependencies, linters or test runners.
2. **Small, focused edits.** Do not rewrite a working file.
3. **Respect load order.** New scripts are classic scripts, added before `app.js`.
4. **Escape all user text** with `escapeHTML()`; store data through `Store`, never raw `localStorage`.
5. **Use existing CSS variables and classes** — never hardcode colours.
6. **Test manually** in Live Server: every view, all six themes, reduced motion, and offline mode.

See [`AGENTS.md`](AGENTS.md) for the full architecture and coding rules.

---

## Acknowledgements

- [MediaPipe FaceMesh](https://github.com/google-ai-edge/mediapipe) and
  [TensorFlow.js](https://www.tensorflow.org/js) for on-device vision
- [Pyodide](https://pyodide.org/) for Python in the browser
- [Font Awesome](https://fontawesome.com/), [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk)
  and [JetBrains Mono](https://www.jetbrains.com/lp/mono/) for typography
- [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference) for music playback

---

<p align="center"><i>Built with plain HTML, CSS and JavaScript — because focus should not need a build step.</i></p>
