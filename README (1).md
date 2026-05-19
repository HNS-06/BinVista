# ⬡ BinVista — Binary Intelligence Platform

<div align="center">

![BinVista](https://img.shields.io/badge/BinVista-v2.0-00c8ff?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iIzAwYzhmZiIgZD0iTTEyIDJMMyA3bDkgNSA5LTV6TTMgMTdsOSA1IDktNW0tOSA1VjdsOSA1djEwIi8+PC9zdmc+)
![HTML5](https://img.shields.io/badge/HTML5-Single--File-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Canvas](https://img.shields.io/badge/Canvas-2D%20Physics-00ffa3?style=for-the-badge)
![Web Audio](https://img.shields.io/badge/Web%20Audio-API-b06cff?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**A fully client-side, real-time binary reverse engineering and intelligence platform.**  
*Physics-simulated CFG graphs · Live disassembly · AI threat detection · Alert audio engine*

[Features](#-features) · [Views](#-views) · [Keyboard Shortcuts](#-keyboard-shortcuts) · [Architecture](#-architecture) · [Screenshots](#-screenshots) · [Getting Started](#-getting-started)

</div>

---

## 📖 Overview

BinVista is a **professional-grade binary analysis dashboard** built as a zero-dependency, single-file HTML application. It simulates the full workflow of a real reverse engineering platform — from binary ingestion and Ghidra-powered symbol extraction, through CFG graph visualization and AI-driven threat analysis, to live disassembly browsing with breakpoint support.

Designed for:
- **Malware analysts** who need rapid behavioral pattern recognition
- **Security researchers** performing static and dynamic binary analysis
- **Reverse engineers** navigating complex control flow graphs
- **CTF players** who want a polished triage environment

> **Built by:** Aurovex_Build  
> **Stack:** Pure HTML5 · Vanilla JS · Canvas 2D · Web Audio API · CSS3  
> **Deployment:** Open `index.html` in any modern browser — no server, no build step, no dependencies

---

## ✨ Features

### 🔴 Live Data Engine
- All metrics (function count, CFG node count, threat score, AI confidence, entropy) **fluctuate in real time** with realistic jitter
- New data points stream into the Call Rate and CFG Growth charts every 2 seconds
- Live activity feed appends real-time events to both the sidebar and the Live panel

### ⬡ Physics-Based CFG Graph
- Nodes and edges rendered on HTML5 Canvas with a **full spring-repulsion physics simulation**
- Every node applies mutual repulsion force and spring attraction along edges
- A random node and edge **pulse and animate** every 1.1 seconds — the graph is never static
- Animated dots travel along active edges during pulse events
- **Hover tooltips** reveal function name, address, instruction count, ref count, call count, and size
- **Double-click** any node to jump directly to its disassembly

### 🔊 Web Audio Alert System
- Built on the **Web Audio API** — zero external dependencies, no audio files
- **Alert sound:** descending sawtooth oscillator chord fires on every threat detection
- **Success sound:** ascending sine triad plays when analysis completes
- **Click / nav tones:** subtle feedback on every interaction
- **Breakpoint set tone:** single high triangle wave on breakpoint toggle

### 🤖 AI Threat Analysis Panel
- 5 active threat detections with severity ratings (HIGH / MEDIUM / LOW / INFO)
- Threat items **flash and animate** when re-detected by the live engine
- Click any threat to jump directly to its address in the Disassembly view
- Alert ticker scrolls across the top bar when threats fire

### ⚡ Analyze Sequence
- Multi-stage analysis animation: File validation → Ghidra extraction → CFG generation → AI analysis → Rendering
- Progress bar advances with realistic per-stage timing
- Works for all 4 sample projects (switch via sidebar Recent Projects)

### 🖥 Split View Comparison
- Two independent CFG graphs rendering simultaneously side by side
- Each graph has its own physics simulation — compare two binaries at a glance

### ⌨ Command Palette
- Press `⌘K` / `Ctrl+K` to open a VS Code-style command palette
- Navigate to any view, jump to any of the 15 functions, or trigger actions instantly
- Filterable by command name or address

---

## 🗂 Views

| View | Key | Description |
|------|-----|-------------|
| **Dashboard** | — | 4-panel live chart grid + stats strip |
| **Graph** | `G` | Physics CFG with minimap, zoom, pan, tooltips |
| **Disassembly** | `D` | x86-64 assembly browser with search & breakpoints |
| **Split** | `S` | Side-by-side dual CFG comparison |
| **Strings** | — | Extracted strings with type tagging and search |
| **Imports** | — | DLL import table with SUSP / CRYPTO / NET tags |

### Dashboard Charts

| Chart | Description |
|-------|-------------|
| **Section Entropy** | Bar chart of PE section entropy values; sections above 7.0 flagged as packed |
| **Function Call Rate** | Live line chart of calls/sec, updates every 2 seconds |
| **CFG Growth** | Scrolling line chart of node count over time |
| **Threat Radar** | Hexagonal radar chart across 6 threat categories |

---

## ⌨ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `G` | Open CFG Graph view |
| `D` | Open Disassembly view |
| `S` | Open Split view |
| `⌘K` / `Ctrl+K` | Open command palette |
| `⌘U` / `Ctrl+U` | Re-run analysis on current project |
| `Escape` | Close command palette |
| `+` / `−` | Zoom in / out (graph toolbar) |
| Double-click node | Jump to function disassembly |

---

## 📐 Architecture

```
BinVista/
├── index.html          # Entire application — single self-contained file
└── README.md           # This file
```

### Internal Module Structure (within `index.html`)

```
┌─────────────────────────────────────────────────────────────┐
│                        BinVista App                         │
├───────────────┬─────────────────────┬───────────────────────┤
│   Audio       │   Data Layer        │   UI Layer            │
│  ─────────    │  ────────────────   │  ─────────────────    │
│  Web Audio    │  PROJS[]            │  Topbar + Ticker      │
│  AC context   │  FNS[] (15 fns)     │  Sidebar + Activity   │
│  tone()       │  STRINGS[] (20)     │  6 View panels        │
│  alertSnd()   │  IMPORTS[] (5 DLLs) │  Right panel (4 tabs) │
│  okSnd()      │  AI_ALERTS[]        │  Command Palette      │
│  clickSnd()   │  DA_CACHE{}         │  Analyze overlay      │
├───────────────┼─────────────────────┼───────────────────────┤
│   Physics     │   Chart Engine      │   Live Engine         │
│  ─────────    │  ──────────────     │  ─────────────────    │
│  makeNodes()  │  drawEnt()          │  startLiveEngine()    │
│  makeEdges()  │  drawCC()  (live)   │  EVT[] event pool     │
│  physTick()   │  drawCFG() (live)   │  AEVT[] alert pool    │
│  initGraph()  │  drawRadar()        │  addAct() feed        │
│  liveMutate() │  updateCC/CFG()     │  pushTicker()         │
└───────────────┴─────────────────────┴───────────────────────┘
```

### Physics Simulation

The CFG graph uses a **force-directed layout** with three forces:

1. **Repulsion** — every node pair repels each other (`force = 2400 / d²`)
2. **Spring attraction** — edges pull connected nodes toward a target distance of 120px
3. **Center gravity** — mild pull toward canvas center to prevent drift

Velocity is damped by `0.87` per tick to achieve stable convergence. Physics runs every 2nd animation frame for performance.

### Disassembly Generation

Each function's disassembly is **procedurally generated** from a pool of 16 x86-64 instruction templates at startup and cached in `DA_CACHE`. Instructions include:

- Proper prologue/epilogue (`push rbp` → `mov rbp, rsp` → `sub rsp, N` → `add rsp, N` → `pop rbp` → `ret`)
- Realistic byte sequences, labeled basic blocks (`loc_XXXXXXXX:`), and inline comments
- Suspicious instructions (e.g. `call IsDebuggerPresent`, `xor rax, rax`) are highlighted in red with `; ⚠ suspicious`

---

## 🧠 Simulated Analysis Pipeline

When you click **ANALYZE**, BinVista runs a 5-stage animated pipeline:

```
Stage 1  ──  File validation & header parsing      (0.11s)
Stage 2  ──  Symbol extraction (Ghidra engine)      (0.72s)
Stage 3  ──  Control flow graph generation          (~0.43s)
Stage 4  ──  AI pattern analysis                    (~1.19s)
Stage 5  ──  Graph rendering & layout               (~0.33s)
```

On completion:
- All 4 dashboard charts render with project-specific data
- Stats strip populates and begins live fluctuation
- CFG graph initializes with physics simulation
- Live engine starts streaming events and alerts
- Alert ticker activates on first threat detection

---

## 📦 Sample Projects

BinVista ships with 4 pre-configured sample projects:

| Project | Format | Arch | Entropy | Threats | Functions |
|---------|--------|------|---------|---------|-----------|
| `malware_sample.exe` | PE32+ | x86-64 | **7.82** (Packed) | **4** | 247 |
| `firmware_v2.bin` | ELF32 | ARM v7 | 5.12 | 1 | 103 |
| `libcrypto.so` | ELF64 | x86-64 | 6.44 | 2 | 389 |
| `loader.elf` | ELF64 | x86-64 | 4.88 | 0 | 38 |

Switch between them via the **Recent Projects** section in the sidebar.

---

## 🔍 Threat Detection Categories

BinVista's AI panel flags the following threat classes:

| Severity | Threat | Address | Description |
|----------|--------|---------|-------------|
| 🔴 HIGH | Ransomware Pattern | `0x14000a2fc` | AES-256 loop + file enumeration |
| 🔴 HIGH | Anti-Debug Routine | `0x140003b10` | `IsDebuggerPresent` + `NtQueryInformationProcess` chain |
| 🟡 MEDIUM | C2 Beacon Logic | `0x140007c44` | HTTP POST to hardcoded IP, 30s poll interval |
| 🟢 LOW | Persistence (Registry) | `0x14000e120` | `HKCU\...\Run` key write |
| 🔵 INFO | XOR String Obfuscation | `0x140001a88` | Dynamic XOR decryption with rotating key |

---

## 🚀 Getting Started

### Requirements

- Any modern browser: Chrome 90+, Firefox 88+, Edge 90+, Safari 15+
- No internet connection required after initial font load
- No Node.js, no npm, no build tools

### Run Locally

```bash
# Clone or download
git clone https://github.com/aurovex/binvista.git
cd binvista

# Just open the file
open index.html
# or
start index.html        # Windows
xdg-open index.html     # Linux
```

### Deploy to Web

Drop `index.html` onto any static host:

```bash
# Vercel
npx vercel --prod

# Netlify drag-and-drop
# → netlify.com/drop → drag index.html

# GitHub Pages
# → Push to repo → Settings → Pages → Deploy from branch
```

---

## 🛠 Customization

### Adding Real Functions

Replace the `FNS[]` array entries with actual function metadata from your analysis tool:

```javascript
const FNS = [
  { nm: 'my_function', type: 'sus', addr: '0x400100', instrs: 42, refs: 3, calls: 2, sz: '336B', tag: 's' },
  // type: 'entry' | 'sus' | 'cry' | 'net' | ''
  // tag:  's' (suspicious) | 'c' (crypto) | 'n' (network) | ''
];
```

### Adding Real Strings

Replace `STRINGS[]` with output from `strings` or Ghidra's string extractor:

```javascript
const STRINGS = [
  { addr: '0x403000', val: 'http://c2.evil.com/beacon', type: 'url', sz: 26 },
  // type: 'url' | 'sus' | 'key' | 'net' | 'plain'
];
```

### Connecting a Real Backend

BinVista's live engine polls `S.timers` intervals. Replace the simulation with real API calls:

```javascript
// Replace startLiveEngine() timer with:
S.timers.push(setInterval(async () => {
  const data = await fetch('/api/live-stats').then(r => r.json());
  document.getElementById('sfn').textContent = data.functions;
  document.getElementById('snodes').textContent = data.nodes;
  // ...
}, 2000));
```

---

## 🎨 Design System

BinVista uses a dark terminal aesthetic with a deliberate color vocabulary:

| Token | Value | Usage |
|-------|-------|-------|
| `--c` | `#00c8ff` | Primary cyan — active states, CFG edges, links |
| `--g` | `#00ffa3` | Green — entry nodes, success, low severity |
| `--r` | `#ff4b6e` | Red — threats, suspicious nodes, high severity |
| `--a` | `#ffb347` | Amber — network nodes, warnings, medium severity |
| `--p` | `#b06cff` | Purple — crypto nodes, AI badge, call edges |
| `--t1` | `#e0f0ff` | Primary text |
| `--t2` | `#5f8aab` | Secondary text |
| `--t3` | `#2e4a62` | Muted text, labels |

**Typography:**
- `Syne 800` — Display headings, logo, stat values
- `Space Mono` — UI labels, navigation, code-adjacent text
- `DM Mono` — Assembly, addresses, hex values, monospace content

---

## 🗺 Roadmap

- [ ] **Real binary upload** — parse ELF/PE headers in-browser via WebAssembly
- [ ] **Ghidra REST bridge** — connect to a local Ghidra server for real disassembly
- [ ] **Collaboration mode** — WebSocket-based shared analysis sessions
- [ ] **Export** — save CFG as SVG, disassembly as text, full report as PDF
- [ ] **Memory view** — live hex dump panel with ASCII sidebar
- [ ] **Decompiler tab** — pseudo-C output alongside disassembly
- [ ] **Yara rule editor** — write and test Yara rules against loaded strings
- [ ] **Dark/light theme toggle**
- [ ] **Plugin API** — hook into the analysis pipeline with custom detectors

---

## 🤝 Contributing

```bash
# Fork the repo, then:
git checkout -b feature/my-improvement
# Make changes to index.html
git commit -m "feat: add memory hex view"
git push origin feature/my-improvement
# Open a Pull Request
```

All contributions welcome — bug reports, feature requests, and PRs.

---

## 📄 License

```
MIT License

Copyright (c) 2026 Aurovex_Build

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

## 👤 Author

**Aurovex_Build**  
Binary intelligence tools · Embedded systems · Full-stack engineering

---

<div align="center">

Built with precision. No frameworks harmed.

**⬡ BinVista** — *See inside every binary.*

</div>
