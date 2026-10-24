# Dexturr Tweaks

A Windows performance tool for competitive players, built around Fortnite. It applies documented Windows, graphics
driver and game settings that cut stutter and input delay. Every change can be undone, and a built-in Frame Test
measures whether it actually helped.

> **Beta.** This repository hosts the installer and the update feed only. The app finds new versions here and installs
> them itself.

---

## Download

Get **`DexturrTweaks-Setup-<version>.exe`** from the latest release under
[**Releases**](../../releases/latest).

Each release has three files:

| File | What it is |
|---|---|
| `DexturrTweaks-Setup-<version>.exe` | The installer. This is the only file you download. |
| `latest.yml` | The update feed the app reads. It holds the version, the installer's SHA-512 hash, and Dexturr's signature. |
| `DexturrTweaks-Setup-<version>.exe.blockmap` | Lets the app download only the parts of the installer that changed. |

**Requirements:** Windows 10 version 22H2 or Windows 11, 64-bit. A few tweaks are Windows 11 only and say so in the
app. Installing needs administrator approval.

**"Windows protected your PC":** the beta installer isn't code-signed with a paid certificate yet, so SmartScreen
warns about it. Click **More info → Run anyway**. Every release lists its SHA-256 hash in the release notes, so you
can check your download first (PowerShell):

```powershell
Get-FileHash .\DexturrTweaks-Setup-<version>.exe -Algorithm SHA256
```

---

## What it does

- **Tweaks:** 55 settings for Windows, NVIDIA and Fortnite, each with what it changes, why, and a source. Examples:
  power plan and processor core parking, Game Mode, background recording, network adapter interrupt moderation,
  Fortnite's processor priority, and NVIDIA's power and shader-cache settings. There are also privacy settings
  (telemetry, ads) and a **Safety** page that puts back updates, security fixes and features that other tweak tools
  turned off. The **Your setup** page suggests the tweaks that fit your hardware.
- **Frame Test:** records every frame with Intel PresentMon. It reports average FPS, 1% and 0.1% lows, spikes,
  PC latency, and how busy the graphics card was, which shows whether the processor or the graphics card is the limit.
  Replay mode compares runs on the exact same fight. The **Stutter Detective** shows which programs were busy when
  the game stuttered.
- **Tournament Mode:** before a match it closes launchers and apps that update or pop up mid-game, and moves VPN
  adapters out of the way. Afterwards it puts everything back, and its checklist links straight to any fix.
- **GPU Overclock:** low, medium and high presets through MSI Afterburner (installed separately). Voltage and power
  limits are never touched, and **Return to normal** restores your own Afterburner profiles exactly.
- **BIOS Tuning:** the BIOS settings that help processor-heavy games, for your motherboard and in order of risk:
  BIOS update, XMP/EXPO, efficiency cores, Hyper-Threading/SMT, memory timings. It restarts straight into the BIOS
  with the menu path on screen, and checks afterwards that each step took effect. The processor is never overclocked.
- **Setup check, network tests, profiles:** checks the setup for your monitor, drivers and processor, tests
  bufferbloat and lag spikes, and saves and loads sets of tweaks.

## Safety

- **Everything is reversible.** Before changing a setting, Dexturr records its original value in a journal under
  `%ProgramData%\Dexturr`. **Undo** (per tweak, or everything at once) writes the original back, even after a crash or
  power loss halfway through.
- **It checks, not assumes.** A tweak shows as *On* only when the setting really is on. Settings that Windows or your
  hardware lock are reported as such, not counted as applied.
- **Nothing hidden.** It doesn't remove Windows components, disable security features or edit game files beyond
  Fortnite's own settings file, which is backed up first.
- **Updates are signed.** The app installs an update only when its signature matches Dexturr's update key, and it
  deletes anything else unopened.

## Privacy

There are no accounts, telemetry or ads. The app goes online only to check this repository for updates (you can turn
that off), for network tests you start yourself, and when you click **Send my results** under a proof card. That
sends one before/after comparison (a random install ID, your processor and graphics card, and the Frame Test
numbers; never your name, email or license key) after showing you exactly what will be sent. It's used to see which
PCs Dexturr helps most and to make it better. Logs stay on your PC in `%LOCALAPPDATA%\Dexturr\logs`, and you
decide whether to send them when reporting a problem.

## How it's built

| Part | Built with |
|---|---|
| App window | [Electron](https://www.electronjs.org/) (JavaScript). Runs without administrator rights. |
| Helper | C# on .NET 10. Makes the actual system changes. It runs elevated only while needed and only accepts requests from the app over a private, authenticated named pipe. |
| Frame recording | [Intel PresentMon](https://github.com/GameTechDev/PresentMon) (MIT license), shipped unmodified with the app. |
| Overclocking | Reads and writes MSI Afterburner's own profile files. Afterburner itself isn't included. |
| Installer and updates | [electron-builder](https://www.electron.build/) (NSIS installer) and electron-updater, plus Dexturr's own Ed25519 update signatures. |
| Licenses | Offline Ed25519-signed keys for Pro. Activation sends nothing anywhere. |

Before a release, the helper's automated tests (over a thousand) and the app's tests, type checks and catalog
validation all have to pass.

## Reporting a problem

Open an [issue](../../issues) with:
- what you clicked and what happened (a screenshot helps)
- your Windows version and your processor and graphics card
- the log files from `%LOCALAPPDATA%\Dexturr\logs`, if you're comfortable sharing them

## Uninstalling

**Settings → Apps → Installed apps → Dexturr Tweaks → Uninstall.** Undo your tweaks in the app first if you want
Windows back exactly as it was. The uninstaller also offers to undo them.

---

© 2026 Dexturr. All rights reserved. The source code isn't public. Third-party components keep their own licenses.
