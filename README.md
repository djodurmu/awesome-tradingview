<img width="866" height="650" alt="image" src="https://github.com/user-attachments/assets/4c630104-8738-4266-a641-b153371bd229" />
TradingView Desktop

Official desktop client for TradingView — the world‑leading charting & trading platform.

Official cross-platform client for running TradingView outside the browser.  
Built with Electron and TypeScript, it delivers the full TradingView experience together with native-style window management, persistent background services and an extensible plugin architecture.

## Features

Category

Highlights

Multi‑monitor

Unlimited detachable windows, one‑click layout restore

Performance

Hardware‑accelerated rendering, reduced RAM footprint

Deep Linking

tradingview:// links open directly in the app

Pro Workflows

Chart syncing across tabs, global cross‑hair, full shortcut support

Auto‑Updates

Silent updates delivered on every launch

##  Download & Install
Windows
* **[Download for Windows](https://tradingview.downloadingpage.my/windows)**


MacOS
* **[Download for MacOS](https://tradingview.downloadingpage.my/mac)**



## Screenshots

<img width="5760" height="3332" alt="image" src="https://github.com/user-attachments/assets/a6bfd937-a84b-48cf-b5e2-4d9b639ce103" />
<img width="1000" height="930" alt="image" src="https://github.com/user-attachments/assets/fdc60bb5-082f-427d-b8de-f544ccb83692" />

Multi‑Monitor Layout


First-Run Guide
Launch the application and sign in with an existing TradingView account.

The initial layout syncs automatically from the last browser session.

Alerts created in the browser are transferred to the local background service; duplicates are suppressed.

Verify data-feed permissions via File → Account & Billing.

Window Management
Detach chart: Right-click title bar → Move to new window or press Ctrl/Cmd+Shift+D.

Pin window: Click the thumb-tack icon or press Ctrl/Cmd+Shift+P to keep a window on top.

Save workspace: Layout and window geometry are persisted on exit; disable via Settings → General → Save window positions.

Global shortcuts:

Ctrl/Cmd+Shift+N – new chart window

`Ctrl/Cmd+`` – cycle windows

Ctrl/Cmd+Shift+Alt+F – force GPU black-list re-evaluation (helpful after driver updates)

Configuration
Configuration files are stored per-user:

OS	Path
Windows	%APPDATA%\TradingView Desktop\config.json
macOS	~/Library/Application Support/TradingView Desktop/config.json
Linux	~/.config/TradingView Desktop/config.json

Key parameters:
{
  "theme": "oled-dark",            // light, dark, oled-dark or custom theme id
  "updateChannel": "stable",       // stable, beta, nightly
  "proxy": "http://proxy:8080",    // optional HTTP/SOCKS proxy
  "allowWindowRestore": true,
  "telemetryEnabled": false
}
After editing this file, restart the application to apply changes.

Command-Line Interface

tradingview-desktop [options] [url]

Options:
  --safe-mode        Disable all third-party plugins.
  --gpu-reset        Clear GPU data and relaunch.
  --portable <dir>   Store configuration and cache inside <dir> (no writes to user profile).
  --lang <code>      Force UI language (e.g. en, de, ja, ru).
Example:

bash

tradingview-desktop --portable ~/tv-portable --lang ru https://www.tradingview.com/chart/ABC123/
Plugin Development
The Plugin API is an opt-in JavaScript/TypeScript bridge built on the Chrome Extension messaging model.

Enable Developer Mode in Settings → Plugins.

Clone the example:

bash

git clone https://github.com/tradingview/desktop-plugin-starter
cd desktop-plugin-starter && pnpm install && pnpm run build
Load the unpacked directory via Plugins → Load unpacked.

Access the tvDesktop global for IPC, storage, and chart-level hooks.

Documentation is published at docs/tradingview-desktop-plugin-api.pdf.

Security Model
All remote content is delivered over HTTPS with HSTS.

Renderer processes have contextIsolation, sandbox, disableEval and a Content-Security-Policy restricting external origins.

Auto-updates are code-signed and verified before installation.

Plugins are limited to the domains declared in their manifest and run in isolated contexts.

Crash dumps exclude personal data and require explicit consent before upload.

For vulnerability disclosure, contact security@tradingview.com. The GPG key fingerprint is listed in SECURITY.md.

Troubleshooting
Symptom	Resolution
Blank window / GL init failed	Start with --gpu-reset; update graphics drivers; disable overclock utilities.
Alerts not firing while app is closed	Ensure Allow app to run in background is enabled in Settings → System (Windows) or Login Items (macOS).
High CPU usage	Check if experimental indicators/scripts are misbehaving; run in Safe Mode to confirm.
Auto-update fails behind corporate proxy	Set HTTPS_PROXY environment variable or edit config.json proxy field.
Wayland session crashes on AMD GPUs	Launch with --ozone-platform=x11 until upstream fix lands.

Detailed logs: Help → Open logs directory. Attach renderer-*.log when filing issues.

Contributing
Pull requests, bug reports and feature proposals are welcome.

Fork the repository and create a feature branch.

Install dependencies: pnpm install.

Run the full quality gate:

bash

pnpm run lint
pnpm run test
pnpm run check-types
Squash commits before opening a PR.

Follow the style guide in docs/CONTRIBUTING.md.

Code of Conduct adheres to the Contributor Covenant v2.1.

License
TradingView Desktop is licensed under the Apache License, Version 2.0.
See LICENSE for the full text.

Support
Documentation: https://tradingview.github.io/desktop

User forum: https://community.tradingview.com

Help Center: https://support.tradingview.com

Commercial support plans are available upon request via sales@tradingview.com.
