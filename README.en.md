# Kirara_Server_APP

> A WinUI 3-based Windows desktop client for unified access to and management of the various application services on Kirara_Server.
> Through the three-step model **Create Instance → Select Scheme → Link Token**, it delivers configure-once, multi-service direct access, and automatic authenticated login.

<div align="center">

![Version](https://img.shields.io/badge/version-1.6.6-blue) ![Platform](https://img.shields.io/badge/platform-Windows%2010%2B-lightgrey) ![Framework](https://img.shields.io/badge/.NET-10-purple) ![License](https://img.shields.io/badge/license-All%20Rights%20Reserved-red)

</div>

---

## ✨ Features

- 📜 **Software license agreement gate**: the Kirara Server Software License Agreement must be read and accepted before user/service instances can be created; the acceptance state is recorded locally per signed-in account. Unattended instance auto-start is gated the same way — with no acceptance **not a single instance starts**: the app first brings its window back on screen and shows the agreement dialog, and continues the start-up sequence **in place** once accepted (no re-login). You can revoke it at any time under "Settings → Agreement Management" (revoking closes the app immediately)
- 🗂️ **Instance-based management**: distinguishes User Instances from Service Instances; supports renaming, favorites, color tags, import/export, and flexible switching between multiple instances; each instance row in the left bar has a one-click "Close Instance" button (auto-greyed while busy / not started, sharing the same state source as the "My Apps" card on the home page)
- ☁️ **Cloud sync (instances and tokens only)**: the cloud buttons in the left function bar and the Token Manager offer "Sync to cloud / Restore from cloud"; the sync scope is strictly limited to **instances + tokens** — software preferences (theme / app behavior / left bar) and Notification Center service items **never** take part, and restoring an older full backup package will **not** overwrite local settings or notification read states; restore counts are split by category (restored / already present and skipped), and cloud data can be deleted in one click under "Settings → Data Management → Clear Cloud Data"
- 🧩 **Plugin-based Scheme system**: 20+ built-in Schemes covering media, live streaming, cloud drives, email, team collaboration, virtualization, remote desktop, OpenVPN, certificate issuance, game servers, and more; also supports importing third-party executables as Schemes
- 🛒 **App Store**: the topmost entry in the left navigation area — browse / search / download distributable ecosystem resources (Kirara apps / plugins / third-party apps); type filtering + fuzzy keyword search; card-based display of cover, description, and type/version badges; cover readiness gating, in-flight task reuse, and old-cache retention on refresh failure; real-time download progress feedback linked with the Download Manager; the download button supports three states: "Download / Waiting for previous queued tasks to finish... / This plugin is already installed"
- 📦 **Plugin install / uninstall**: the three runtime components FreeRDP, OpenVPN, and WebView2 can be installed/upgraded with one click in the App Store; automatic readiness detection (not installed / installed / outdated); elevated installation uses an "evidence-first" criterion (based on the component actually landing on disk, not on exit codes); WebView2 installation requires no elevation; uninstallation is limited to store-installed components (system-level WebView2 cannot be uninstalled); OpenVPN must be disconnected before uninstallation; FreeRDP uninstallation is refused when its directory is in use; the uninstall entry separates "visible / available"; path safety is confined to the `ThirdAPPs` directory; install/uninstall runs in the background so you can leave the page, and the busy state is automatically restored when you return
- 🖼️ **Store cover caching**: covers are fetched via `302` presigned direct links; ETag comparison + AES-256-GCM encrypted cache (account-isolated); in-flight tasks with the same key are reused; gating can wait for the final result of the current sync; store-origin notifications can display covers in sync, and if the cache misses, they are supplemented in the background
- 🧰 **Store UI experience hardening**: card install/download/uninstall busy states and hover tooltips, separation of the uninstall entry's "visible / available", restoration of background operation states, and a unified result dialog (notification / warning) with brush fallback and window theme following
- 🔗 **Optional third-party component installation linkage**: the installer makes OpenVPN / FreeRDP optional components, unchecked by default; upgrades/silent installs inherit the existing installation state on the machine; if not installed, they can be added later from the App Store
- 🔑 **Token Manager**: centrally manages accounts and certificates; sensitive fields are written to disk after **DPAPI + AES-256-GCM double encryption**; supports manual export/import as well as **QR-code export** — one click pops up a dynamic encrypted QR code for import by scanning on a mobile device
- 📲 **QR-code token export**: each row in the Token Manager has a scan button that pops up a dynamic QR code; the Kirara_Media mobile client can scan it to import the token securely; the QR code auto-refreshes every 15 seconds, the session auto-renews as it nears expiry, and the whole process uses **AES-256-GCM encrypted transmission**; only the account that owns the token can import it, and scans by anyone else are rejected with a security prompt
- 🔐 **Auto-login engine**: a WebView2-based automated login framework with built-in adaptations for multiple Schemes, supporting single-step/step-by-step/delayed login and failure-degradation detection
- 🛡️ **In-house OpenVPN management panel**: integrates the openvpn.exe engine with real-time status, traffic, and log display; .ovpn configurations are uniformly synchronized and distributed by the server
- 🖲️ **Remote desktop input hardening**: embedded FreeRDP / RDP Schemes **silence the host IME** (suppressing local East Asian candidate windows and swallowed keystrokes, decided per thread/window, switching input mode only without changing the language) and bridge the clipboard without stealing focus — background copies no longer activate the host window or push it to the front
- 🔔 **Real-time Notification Center**: SignalR WebSocket real-time push + offline catch-up after disconnection + read receipts; notifications feature feed-card carousels (image/video/HTML); interaction notifications such as likes, replies, and update requests are delivered in real time
- 💬 **Issue comment system**: an embedded comment area on Scheme pages supporting replies and likes, synced across clients; now expanded into a four-tab view: "Community / My Recent Comments / Favorites / Update Requests"
- ⭐ **Favorites system**: one-click favorite/unfavorite in the media comment panel with real-time status sync; "My Favorites" provides centralized management and one-click jump back to the resource page
- 📣 **Community update-request platform**: select a target media library to post update requests and like/interact; admin read feedback is pushed in real time
- 🎮 **Multi-version game resource downloads**: within the game server resource platform, the same resource supports **parallel display of multiple versions** (each version on its own row, newest to oldest by version number), version-level SHA-256 integrity verification, real-time download progress feedback, and one-click open of the containing folder when complete
- 🌈 **Appearance theme system**: follow system / manual light-dark / 48-color Win11-style accent colors, with three-level linkage taking effect instantly; comment avatars automatically refresh to match the light/dark theme
- 🚀 **Quick Start templates**: built-in common service-group templates for one-click application; cover images and templates are managed by the server
- 🛠️ **WebView2 stability hardening**: self-healing from process crashes and automatic rebuild via a white-screen watchdog (limited retries + cooldown), plus double protection against file-dialog reentrancy crashes
- 🖱️ **External file drag & drop**: files can be dragged straight from File Explorer / the desktop into Scheme pages, in-app standalone browser windows, and the Admin Panel (fixing the WinUI 3 issue where external drops are blocked — with a "no entry" cursor — because WebView2 is hosted windowlessly); once accepted, Windows natively forwards the drop to the page, which receives **real file entries**, so in-page drop highlights and upload controls work as usual
- 🖥️ **Immersive interface**: Fluent 2 design language, custom-drawn title bar, DPI adaptation (PerMonitorV2), global busy-state overlay, and page transition animations

## 📋 System Requirements

- **OS**: Windows 10 1809 (10.0.17763) or later; Windows 11 recommended
- **RAM**: 16 GB or more recommended
- **GPU**: an NVIDIA GeForce RTX 2060 or better recommended, with the latest drivers to support AI image super-resolution and HDR
- **Architecture**: x64
- **Runtime**: .NET 10 (the installer usually bundles it or bootstraps it automatically)
- **Administrator privileges**: the app itself runs as a normal user (`asInvoker`, v0.9.1+); the TAP/Wintun virtual adapter required by OpenVPN is created on its behalf by the `KiraraVpnHelper` service (LocalSystem) registered at install time, so **connecting a VPN needs no UAC elevation**. Only installing / upgrading / uninstalling the package itself requires administrator privileges

## ⬇️ Installation and Updates

1. Go to this repository's **Releases** page and download the latest installer (`Kirara_Server_APP_Setup_x64_vX.X.X.exe`)
2. Run the installer and follow the wizard to complete installation
3. After the first launch, register/log in with your Kirara_Server account to start creating your first instance
4. The program has an auto-updater that can automatically uninstall the old version and install the update
5. WebView2 is a required component; OpenVPN / FreeRDP are optional components, unchecked by default during installation, and can be installed or upgraded at any time from the in-app **App Store**
6. Upgrades or silent installs inherit the existing installation state on the machine and do not add or remove any installed third-party components
7. Before creating your first user/service instance you must read and accept the Kirara Server Software License Agreement; the acceptance state is recorded locally per account and can be revoked under "Settings → Agreement Management"

## 🚀 Quick Start

### Three steps to get started

```
① Create Instance  →  ② Select Scheme(s) (multiple allowed)  →  ③ Link Token
```

1. **Create an instance**: in the left function bar, click "Create New User Instance" or "Create New Service Instance", name it, and select Schemes
2. **Link tokens**: link the corresponding account token to each Scheme (create them in advance in the Token Manager)
3. **One-click access**: after applying the instance, the program automatically negotiates the communication method with the server, auto-fills credentials, and attempts to log in; once successful, you can use each Scheme's services directly in tabs

> 💡 You can also use a "Quick Start" template to directly apply an officially preconfigured common service group, saving manual configuration.

### Common operations

- **Instance management**: centrally managed in the left column of the main interface; instances support import/export of `.kirara` files for sharing; each instance row has a one-click "Close Instance" button (auto-greyed while busy / not started)
- **Cloud sync**: the cloud button to the right of the search box in the left function bar (there is one in the Token Manager too) → "Sync to cloud / Restore from cloud", covering **instances and tokens only**; cloud data can be deleted under "Settings → Data Management → Clear Cloud Data"
- **License agreement**: must be read and accepted when creating your first instance; "Settings → Agreement Management" lets you revoke an accepted agreement (the button is greyed out while not accepted)
- **External drag & drop**: drag files from File Explorer / the desktop directly onto Scheme pages, standalone browser windows, or the Admin Panel to upload them — no copy-paste needed
- **Token management**: create/edit/delete tokens with batch export and import; the application credentials associated with tokens are stored fully encrypted
- **QR-code token export**: in the Token Manager, click the "Scan" button on the right side of a token row to pop up a dynamic QR code — point the "Scan to Import" feature of the Kirara_Media mobile client at it to complete the import automatically (the phone and PC must be logged into the same account)
- **App Store**: the "App Store" entry at the top of the left navigation area — browse/search ecosystem resources and download and install plugins (FreeRDP / OpenVPN / WebView2); installed plugins can be uninstalled with one click; the download button supports three states: "Download / Waiting for previous queued tasks to finish... / This plugin is already installed"
- **Plugin uninstallation**: plugin cards always show an uninstall icon, available only for store-installed components; WebView2 cannot be uninstalled; OpenVPN must be disconnected before uninstallation; if the FreeRDP directory is in use, you will be prompted to close the relevant programs first
- **Scheme operations**: within a tab you can refresh (preserving the session), hard reload (clearing cookies/cache), and relink the token
- **Notification Center**: expand the badge in the bottom-right corner; it contains three tabs: "Feed / Service Items / Log Items"
- **Appearance settings**: the settings page lets you switch light/dark themes and accent colors, with follow-system support

Core design:

- **Plugin-based Schemes**: the `IScheme` interface + a compile-time-safe Guid→Type mapping, supporting five presentation modes: WebView2 / RDP / CustomControl / Overlay / ExternalProcess
- **Replaceable remote service integration**: all remote services (authentication, notifications, comments, templates, feeds, etc.) implement existing interfaces and can be integrated by replacing the DI registration, with automatic degradation when offline
- **Incremental sync**: Scheme URLs, feed cards, and .ovpn configurations all use version numbers + client-reported incremental sync, keeping network overhead minimal
- **Credential security**: sensitive credentials such as RefreshToken are only written to disk in DPAPI + AES-256-GCM encrypted form; AccessToken is kept in memory only and refreshed automatically before expiry
- **QR-code export security**: the QR code uses server-side one-time session key exchange + AES-256-GCM encryption, with a random nonce per frame; even if the QR code is screenshotted, a non-owner account cannot decrypt or import it
- **Store plugin pipeline**: store resources and the game resource platform share `GET /api/downloads` (isolated by the `store` first-segment namespace, with no cross-talk); covers use `302` presigned direct links + ETag comparison + account-isolated AES-256-GCM encrypted cache, in-flight task reuse, and cover readiness gating; plugin installation uses elevation + an evidence-first criterion (based on the component actually landing on disk, not on exit codes), and WebView2 installation requires no elevation; uninstallation is limited to store-installed components (`IsAppOwned`), completing termination of this program's process / `msiexec /x` / directory deletion within a single UAC prompt, with path safety confined to the `ThirdAPPs` directory
- **Optional third-party component installation linkage**: the installer and store plugins point to the same `{app}\ThirdAPPs\{plugin}`; if not installed, it can be added from the store, and upgrades inherit the existing installation state
- **Cloud sync package boundary**: `ExportCloudSyncBundle` / `ImportCloudSyncBundle` carry "instances + tokens" only; they share the same import core as the v2 full package in Settings (instances + tokens + preferences + notifications) but keep separate semantics; any preferences/notifications left in an instance package by an older version are ignored outright; the default token merges ApiKey / ServerUrl only (its account credentials are never overwritten, so local auto-login is not clobbered), and nothing is re-encrypted or written when the plaintext is unchanged — hence "upload then immediately restore" leaves local ciphertext untouched
- **License agreement gate**: `LicenseAgreementService` persists the acceptance state per account (`license.agreed.{userId}`) and offers a "grant event + polling" double-safeguard wait. User-initiated entries only intercept without waiting ("View License Agreement" ≠ acceptance); unattended auto-start uses "intercept + wait": for a silent start (window parked off screen) the window is first restored on screen before the dialog appears, then the sequence resumes in place on acceptance, while refusal / timeout abandons that auto-start
- **WebView2 external drag & drop**: the WinUI 3 WebView2 is hosted windowlessly (visual hosting), so drop hit-testing lands on the XAML island — the XAML drag-drop layer must explicitly accept it (`AllowDrop=true` plus `AcceptedOperation=Copy`, set on every `DragOver`, for file drags only); once accepted, WinUI natively forwards the drop to Chromium, so the host must not read and re-inject file content itself (that would create a second path and duplicate uploads); `WebView2ExternalDropBridge` attaches idempotently (weak-reference table) and re-attaches after a crash-healing rebuild

## ❓ FAQ

<details>
<summary><b>Mobile scan-to-import fails / shows rejected?</b></summary>

When importing by scanning on a phone, the KIRARA account logged in on the phone must match the account that exported the token on the desktop — a token can only be imported into the account that owns it. In addition, the QR code refreshes about every 15 seconds; if you see "QR code expired", wait for the pattern on the desktop to refresh and scan again immediately, keeping the desktop scan window open.
</details>

<details>
<summary><b>It says "Administrator privileges required"?</b></summary>

The app itself runs as a normal user (`asInvoker`, v0.9.1+), and the virtual network adapter needed by OpenVPN is created on its behalf by the `KiraraVpnHelper` service (LocalSystem) registered at install time — **connecting a VPN needs no UAC elevation**. Only installing / upgrading / uninstalling the package itself requires administrator privileges, so right-click the installer and choose "Run as administrator".
</details>

<details>
<summary><b>Does cloud sync upload my settings and notifications?</b></summary>

No. Cloud sync moves "instances + tokens" only: software preferences (theme / app behavior / left bar) and Notification Center service items never take part, and when restoring from the cloud those fields — even if present in the package (a full backup uploaded by an older version) — are ignored, so local settings and notification read states are never overwritten. Cloud data already stored can be deleted under "Settings → Data Management → Clear Cloud Data".
</details>

<details>
<summary><b>It says "software license agreement not in effect", and instances don't auto-start?</b></summary>

Without accepting the Kirara Server Software License Agreement, no instance starts: creating / opening an instance shows the agreement dialog first, and unattended auto-start (including silent start) brings the window back on screen and then shows the dialog — accepting continues the start-up **in place**, while refusing or timing out abandons that auto-start. Viewing and revoking are under "Settings → Agreement Management".
</details>

<details>
<summary><b>Can I drag files into the page straight from File Explorer?</b></summary>

Yes. Scheme pages, in-app standalone browser windows, and the Admin Panel all accept files dragged from File Explorer / the desktop (the cursor shows "Copy" while dragging, and in-page drop highlights and upload controls work as usual). If a particular page does not react to a drop, send us the Scheme page address so we can add in-page forwarding.
</details>

<details>
<summary><b>OpenVPN / FreeRDP is not installed or outdated?</b></summary>

Open the in-app "App Store" and search for the corresponding plugin to install/upgrade it with one click; the store automatically detects the component state on the machine (not installed / installed / outdated), so there is no need to download installers manually. WebView2 is a system-level component and cannot be uninstalled; OpenVPN must be disconnected before uninstallation; if the FreeRDP directory is in use during uninstallation, close the relevant programs first.
</details>

<details>
<summary><b>Store covers don't show up immediately?</b></summary>

On first use, covers are downloaded via `302` presigned direct links and written to an account-isolated encrypted cache; if the cache misses, the store first shows a type icon and syncs in the background, refreshing automatically when done. If the refresh fails, the old cache is retained and the cover will not flash blank.
</details>

<details>
<summary><b>Can I still use it when the network is down?</b></summary>

The app supports offline degradation: local cache, local authentication, and local comments serve as fallbacks; once back online, remote sync and real-time push resume automatically.
</details>

<details>
<summary><b>Where are token/instance data stored?</b></summary>

Data is stored in the `%APPDATA%/Kirara/` directory. Sensitive fields such as tokens are encrypted — do not copy this directory directly to others. Since v1.5.0, data is stored isolated per account (a `users/{account ID}/` subdirectory plus a global layer), and accounts cannot see each other's data when switched. If the software's data becomes abnormal, first try "Clear Cache" (Scheme caches only; account data is unaffected) or "Clear Local Data" (account data and all local caches; irrevocable — export a backup first) under "Settings → Data Management"; if that still does not help, uninstall and reinstall the software.
</details>

## 📄 License

This software is **closed-source proprietary software. All Rights Reserved.**

- Without authorization, decompiling, distributing, modifying, or republishing the software is prohibited
- The copyrights of the third-party open-source components used by the software belong to their respective authors

## 🙏 Acknowledgements

- [LiteDB](https://github.com/litedb-org/LiteDB)
- [OpenVPN](https://github.com/OpenVPN)
- [FreeRDP](https://github.com/FreeRDP)
- [QRCoder](https://github.com/codebude/QRCoder)

---

<div align="center">Made with ♥ by SenVenth AC</div>
