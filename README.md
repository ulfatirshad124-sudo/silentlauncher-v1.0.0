<div align="center">

# SILENT LAUNCHER

**Premium red & black game / app launcher for Windows**

`DEVELOPER: SILENT GAMR`

![Platform](https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-black?style=flat-square&color=FF0000)
![.NET](https://img.shields.io/badge/.NET-8.0-black?style=flat-square&color=FF0000)
![UI](https://img.shields.io/badge/UI-WPF%20%C2%B7%20MVVM-black?style=flat-square&color=FF0000)

</div>

---

## About
SILENT LAUNCHER keeps all your games and programs in one sleek, lightweight library. Import any `.exe`, launch it in one click, track it, close it, and show a small **DEVELOPER: SILENT GAMR** watermark on screen while you play.

Only the **original path** of each EXE is stored - files are never copied.

## Features
- **Dashboard** - welcome card, total / running counters, recently added, recently opened, instant search
- **EXE library** - add one or many EXEs, rename, custom icon, launch arguments, remove, missing-file detection
- **Launching** - one-click launch (UAC prompts supported), process tracking, graceful close, crash and error messages
- **Window hosting** - optional **EMBED** mode for compatible apps, automatic fallback to normal external launch
- **Watermark overlay** - bright-red, transparent, click-through `DEVELOPER: SILENT GAMR` text; on/off, opacity, size, 4 corner positions
- **Settings** - accent colour, startup options, library management, reset; everything saves automatically
- **Lightweight** - low idle CPU/RAM, icons load in the background, single instance

## Screenshots
> Add your own screenshots here after the first run, e.g. `![Dashboard](docs/dashboard.png)`

## Download
Go to the **Actions** tab, open the latest successful run, and download the **SilentLauncher** artifact (contains `SilentLauncher.exe`).
No .NET install is needed - the EXE is self-contained.

> Windows SmartScreen may warn about an unsigned app: click **More info -> Run anyway**.

## Quick start
1. Run `SilentLauncher.exe`.
2. Click **ADD APPLICATION** and pick an `.exe`.
3. Press **LAUNCH** on its card (or **EMBED** to try hosting it inside the launcher).
4. Tune the watermark in **Settings**.

## Build from source
Requirements: Windows 10/11, [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) (or Visual Studio 2022 with *.NET desktop development*).

```bash
git clone https://github.com/YOURNAME/SilentLauncher.git
cd SilentLauncher
dotnet run --project SilentLauncher/SilentLauncher.csproj
```

Publish a single-file EXE:

```bash
dotnet publish SilentLauncher/SilentLauncher.csproj -c Release -r win-x64 --self-contained true ^
  -p:PublishSingleFile=true -p:IncludeNativeLibrariesForSelfExtract=true -p:EnableCompressionInSingleFile=true -o publish
```

Output: `publish/SilentLauncher.exe` (or just run `publish.bat`).

## Automatic build (GitHub Actions)
Create `.github/workflows/build.yml`:

```yaml
name: Build SilentLauncher
on:
  push:
    branches: [ main ]
  workflow_dispatch:
jobs:
  build:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.0.x'
      - name: Publish single-file EXE
        run: >
          dotnet publish SilentLauncher/SilentLauncher.csproj
          -c Release -r win-x64 --self-contained true
          -p:PublishSingleFile=true
          -p:IncludeNativeLibrariesForSelfExtract=true
          -p:EnableCompressionInSingleFile=true
          -o publish
      - uses: actions/upload-artifact@v4
        with:
          name: SilentLauncher
          path: publish/SilentLauncher.exe
```

## Where data is stored
`%AppData%\SilentLauncher\` - `library.json`, `settings.json`, `icons\`, `log.txt`

## Good to know
- **Embedding is best-effort.** Exclusive-fullscreen/DirectX games, apps running as administrator, and store launchers that spawn another process (Steam, Epic, Battle.net...) can't be embedded and run externally instead.
- **Stub launchers:** if an EXE only starts another program and exits, the launcher can track only the process it started.
- **Overlay:** shows over windowed and borderless games; exclusive fullscreen may hide it. It is a normal top-most window - it does **not** inject, hook, or read game memory, and does not touch anti-cheat. If a game blocks overlays, switch off its **WATERMARK OVERLAY** toggle and the watermark hides while it runs.

## Project structure
```
SilentLauncher/
 |- App.xaml(.cs)             startup, single instance, crash guard
 |- Themes/Theme.xaml         black & red theme and control styles
 |- Models/                   AppEntry, AppSettings
 |- ViewModels/               MVVM view models and commands
 |- Views/                    MainWindow, dialogs, WatermarkWindow
 |- Controls/HostedAppControl window hosting (HwndHost)
 |- Services/                 process, storage, icons, watermark, startup
 |- Interop/NativeMethods.cs  Win32 P/Invoke
```

## Tech
C# · .NET 8 · WPF · MVVM · System.Text.Json · Win32 window APIs

---

<div align="center">

**SILENT LAUNCHER** - made by **SILENT GAMR**

</div>
