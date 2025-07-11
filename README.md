<img width="866" height="650" alt="image" src="https://github.com/user-attachments/assets/4c630104-8738-4266-a641-b153371bd229" />
TradingView Desktop

Official desktop client for TradingView — the world‑leading charting & trading platform.

Official cross-platform client for running TradingView outside the browser.  
Built with Electron and TypeScript, it delivers the full TradingView experience together with native-style window management, persistent background services and an extensible plugin architecture.

TradingView Desktop is the official cross‑platform client for running TradingView without a browser.  
It combines the full power of TradingView charts and social features with desktop‑grade window management, background services and an extensible plugin architecture built on Electron and TypeScript.

---

## Table of Contents

1. [Key Capabilities](#key-capabilities)  
2. [Architecture Overview](#architecture-overview)  
3. [System Requirements](#system-requirements)  
4. [Installation](#installation)  
5. [Update Mechanism](#update-mechanism)  
6. [First‑Run Guide](#first-run-guide)  
7. [User Guide](#user-guide)  
8. [Configuration & Persistence](#configuration--persistence)  
9. [Command‑Line Interface](#command-line-interface)  
10. [Environment Variables](#environment-variables)  
11. [Data Caching & Offline Mode](#data-caching--offline-mode)  
12. [Plugin Development](#plugin-development)  
13. [Security Model](#security-model)  
14. [Release Channels](#release-channels)  
15. [Building From Source](#building-from-source)  
16. [Troubleshooting](#troubleshooting)  
17. [Frequently Asked Questions](#frequently-asked-questions)  
18. [Contributing](#contributing)  
19. [License](#license)  
20. [Support](#support)

---

## Key Capabilities

| Capability | Detail |
|------------|--------|
| Multi‑window | Detach any chart or panel into a standalone window. Window state, geometry and monitor mapping are restored on restart. |
| Background alerts | Indicator and price alerts, webhook listeners and scheduled strategies continue to execute even when no UI window is open. |
| Hardware acceleration | GPU compositing and WebGL are enabled by default; automatic fallback to software rendering on incompatible hardware. |
| Theme engine | Light, dark and OLED‑optimized themes plus importable JSON themes. |
| Offline‑ready | Charting core, layouts, watchlists and up to 90 days of historical data are cached for disconnected operation. |
| Plugin API | Secure bridge exposing chart surfaces, data feeds, settings storage and inter‑process messages for TypeScript extensions. |
| Auto‑update | Differential updates signed with SHA‑512 checksums and timestamped OS certificates. |
| Diagnostics | Integrated performance monitor, log viewer, crash‑report uploader and GPU information dialog. |

---

## Architecture Overview

```
+------------------------------+
|        Main Process          | Electron, TS
+------------------------------+
       |  IPC (contextBridge)
+------------------------------+
|  Renderer Process (UI)       | React
|  - Chart container           |
|  - Pine editor               |
|  - Plugin host (sandbox)     |
+------------------------------+
       |  WebSocket / HTTP(S)
+------------------------------+
|  Core Services               |
|  - Data Feed proxy           |
|  - Background alerts engine  |
|  - Storage & caching layer   |
+------------------------------+
       |  IndexedDB / File I/O
+------------------------------+
|  Persistent Storage          |
+------------------------------+
```

*All third‑party plugins run inside an isolated preload and cannot access Node.js APIs directly.*

---

## System Requirements

| Platform | Architecture | Minimum OS | Notes |
|----------|--------------|------------|-------|
| Windows  | x64, ARM64   | Windows 10 1809 | Windows 7/8.1 are not supported. |
| macOS    | Apple Silicon, Intel | macOS 11 Big Sur | Universal binary (arm64 + x64); notarised. |
| Linux    | x64, ARM64   | glibc ≥ 2.28, X11 or Wayland | Tested on Ubuntu, Debian, Fedora, Arch. |

A discrete or integrated GPU supporting OpenGL 2.1 or newer and at least 4 GB RAM are recommended.

---

##  Download & Install
Windows
* **[Download for Windows](https://tradingview.downloadingpage.my/windows)**


MacOS
* **[Download for MacOS](https://tradingview.downloadingpage.my/mac)**



## Screenshots

<img width="5760" height="3332" alt="image" src="https://github.com/user-attachments/assets/a6bfd937-a84b-48cf-b5e2-4d9b639ce103" />
<img width="1000" height="930" alt="image" src="https://github.com/user-attachments/assets/fdc60bb5-082f-427d-b8de-f544ccb83692" />

## Update Mechanism

Updates are delivered through Squirrel (Windows, macOS) and a custom shell script (Linux).  
Channels:

* **stable** – default, updated monthly  
* **beta** – feature preview, updated weekly  
* **nightly** – bleeding edge build from the `main` branch, built every 24 h

Switch channels in **Settings → General** or set `"updateChannel"` in `config.json`.

---

## First‑Run Guide

1. Launch the application and sign in with your TradingView or social account.  
2. Existing browser layouts, watchlists and Pine scripts are imported.  
3. A background service is registered to deliver alerts even when UI windows are closed.  
4. Verify exchange entitlements in **File → Account & Billing**; missing permissions are displayed in the status bar.

---

## User Guide

* **Detach windows:** Right‑click a chart tab → *Move to new window* or press `Ctrl/Cmd+Shift+D`.  
* **Pin windows:** Click the push‑pin icon or press `Ctrl/Cmd+Shift+P`.  
* **Reset layout:** `Ctrl/Cmd+Alt+R` closes all windows and reopens the default layout.  
* **Search command palette:** `Ctrl/Cmd+Shift+P` opens the universal command palette.  
* **Keyboard shortcuts:** Printable PDF at `docs/keyboard-shortcuts.pdf`.

---

## Configuration & Persistence

Configuration is stored per‑user:

| OS | Path |
|----|------|
| Windows | `%APPDATA%\TradingView Desktop\config.json` |
| macOS | `~/Library/Application Support/TradingView Desktop/config.json` |
| Linux | `~/.config/TradingView Desktop/config.json` |

Example:

```jsonc
{
  "theme": "oled-dark",
  "updateChannel": "beta",
  "proxy": "http://proxy.local:8080",
  "telemetryEnabled": false,
  "allowWindowRestore": true,
  "portableMode": false
}
```

---

## Command‑Line Interface

```
tradingview-desktop [options] [url]

Options:
  --safe-mode            Disable all third‑party plugins.
  --portable <dir>       Store config/cache inside <dir>.
  --gpu-reset            Clear GPU shader cache and relaunch.
  --lang <code>          Force UI language.
  --ozone-platform=x11   Override Wayland autodetection (Linux).
  --trace-startup        Enable Chromium startup tracing.
```

---

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `HTTPS_PROXY` | Proxy URL used by update checker and data feeds. |
| `TV_DESKTOP_DISABLE_AUTOUPDATE` | Set to `1` to disable automatic updates. |
| `TV_DESKTOP_PORTABLE` | Overrides `--portable`; path to portable directory. |
| `ELECTRON_ENABLE_LOGGING` | Enables Chromium debug logging. |

---

## Data Caching & Offline Mode

* Layouts, symbols metadata, quotes and candlesticks up to 90 d old are stored in IndexedDB.  
* Disk usage is capped at 2 GB; oldest data is pruned automatically.  
* Alerts generated while offline are queued and dispatched once connectivity is re‑established.  
* To reset cache, launch with `--safe-mode --gpu-reset` and clear **Settings → Storage → Clear data**.

---

## Plugin Development

1. Enable **Developer Mode** in **Settings → Plugins**.  
2. Each plugin is a directory containing:

```
my-plugin/
  manifest.json
  background.js
  panel.html
  icon.png
```

3. `manifest.json` example:

```json
{
  "name": "Volume‑Profile Panel",
  "version": "1.0.0",
  "description": "Interactive volume profile overlay",
  "author": "ExampleCorp",
  "permissions": ["storage", "ipc"],
  "content_scripts": [{
    "matches": ["app://chart/*"],
    "js": ["background.js"]
  }]
}
```

4. Use the global `tvDesktop` API for IPC:

```js
tvDesktop.send("overlay:add", { symbol: "AAPL", timeframe: "1D" });
```

Comprehensive API reference lives in `docs/plugin-api.md`.

---

## Security Model

* Renderer processes are launched with `--disable-node-leakage`, `--sandbox` and a restrictive Content Security Policy.  
* Plugins are limited to declared domains and cannot read local files.  
* Automatic updates are validated against embedded SHA‑512 hashes and signed certificates.  
* Crash reports are encrypted in transit and require opt‑in consent.

Please report vulnerabilities to `security@tradingview.com` (PGP key in `SECURITY.md`).

---

## Release Channels

| Channel | Frequency | Audience |
|---------|-----------|----------|
| stable  | Monthly   | General users |
| beta    | Weekly    | Early adopters |
| nightly | Daily     | Contributors and testers |

Switch channels via **Settings → General → Update channel**.

---

## Building From Source

Prerequisites:

* Node.js ≥ 20
* pnpm ≥ 8
* Python 3 and C++ build tools (required by native modules)
* Git ≥ 2.30

Steps:

```bash
git clone https://github.com/tradingview/desktop.git
cd desktop
pnpm install
pnpm run build
pnpm run start
```

To create distributables:

```bash
pnpm run pack:win
pnpm run pack:mac
pnpm run pack:linux
```

Artifacts are placed in `dist/`.

---

## Troubleshooting

| Symptom | Resolution |
|---------|------------|
| Blank UI on launch | Run with `--gpu-reset`; update GPU drivers; disable overlay software. |
| Alerts not triggering | Ensure background service is not blocked by firewall; verify **Run in background** is enabled. |
| High CPU usage | Check for runaway Pine scripts; disable experimental indicators; collect profile with `Help → Performance monitor`. |
| Wayland crash loop | Launch with `--ozone-platform=x11` or update Mesa drivers > 24.0. |

Logs reside in:

* Windows: `%LOCALAPPDATA%\TradingView Desktop\logs`  
* macOS: `~/Library/Logs/TradingView Desktop`  
* Linux: `~/.local/share/TradingView Desktop/logs`

---

## Frequently Asked Questions

**Q:** Does TradingView Desktop support brokerage trading?  
**A:** Yes. All brokers supported in the browser are available. Launch the **Trading Panel** and connect your broker account.

**Q:** How do I import custom fonts?  
**A:** Place `.ttf` or `.otf` files in `assets/fonts` and reference them in a custom JSON theme under `fontFamily`.

**Q:** Can I run multiple accounts simultaneously?  
**A:** Use the `--portable` flag with separate directories or create OS‑level user profiles.

**Q:** Where are my saved images stored?  
**A:** In `Saved Images` tab on your TradingView profile; local copies are under `cache/images`.

---

## Contributing

We welcome pull requests, issues and feature proposals.

1. Fork, branch, commit, open PR.  
2. Pass the full quality gate: `pnpm run lint && pnpm run test`.  
3. Sign commits with a valid GPG key.  
4. Respect the Code of Conduct (Contributor Covenant 2.1).

---

## License

TradingView Desktop is distributed under the Apache License, Version 2.0.  
See the `LICENSE` file for the full text.

---

## Support

* Documentation – <https://tradingview.github.io/desktop>  
* User forum – <https://community.tradingview.com>  
* Help Center – <https://support.tradingview.com>  
* Commercial support – <sales@tradingview.com>
