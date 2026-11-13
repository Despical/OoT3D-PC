# Ocarina of Time 3D on PC (AMD-friendly setup)

How to play the **3DS version** of *The Legend of Zelda: Ocarina of Time 3D* natively on Windows,
with the single-screen TopScreen UI, using the **TriAevum** PC recompilation. Tested and working on
**AMD Radeon** cards.

This repository contains **only documentation**. It does not contain the game, a ROM, the TriAevum
binaries or the TopScreen files. You download those yourself from the links below.

---

## TL;DR

| What | Version | Where |
|---|---|---|
| TriAevum (the PC port) | **v0.6.0-alpha.2b**, Windows x64 ZIP | [GitHub releases](https://github.com/coccofresco/TriAevum/releases) |
| TopScreen texture/patch pack | **2.1.1** (`topscreen211.zip`) | [GameBanana (official)](https://gamebanana.com/mods/695893) · [Google Drive mirror](https://drive.usercontent.google.com/download?id=1YOqjmq4g6HSlSdU1D9Q-M5BOQvbOWZFX&export=download&confirm=t) |
| Game | OoT3D **USA or EUR**, **decrypted** `.3ds` / `.cci` | Dump your own cartridge (see [ROM](#the-rom)) |

Do **not** use the newest TriAevum (Echoes of Time / v0.8.0-beta.1, alpha.3, alpha.3b) on AMD.
[Why?](#why-not-the-latest-version)

---

## What is TriAevum?

[TriAevum](https://github.com/coccofresco/TriAevum) is a native PC **AOT recompilation** of
Ocarina of Time 3D (not an emulator) with a Vulkan renderer. It brings:

- Single-screen UI: a reimplementation of rlgcarrot's [TopScreen](https://gamebanana.com/mods/695893) mod
- Widescreen, configurable FOV, toon shading/outlines, AO, shadows, procedural grass
- 60/90 FPS visual interpolation (game logic stays at 30 Hz)
- Keyboard/mouse and controller support, save states

It has two programs:

- **`TriAevumForge.exe`**: one-time setup. It reads your ROM, applies TopScreen and prepares the game data.
- **`TriAevum.exe`**: the game itself. You launch this from then on.

## Requirements

- Windows 10/11 x64
- A Vulkan-capable GPU with **current drivers**
  (tested on AMD RX 5500 XT / RX 470 class cards)
- A **decrypted** OoT3D ROM, USA or EUR, as `.3ds` or `.cci`.
  Encrypted ROMs and `.cia` files are rejected.

## Installation

1. **Update your GPU driver.** AMD: *AMD Software: Adrenalin Edition → Check for updates*.
2. Download **`TriAevum-v0.6.0-alpha.2b-Windows-x64.zip`** (the release ZIP, *not* GitHub's
   "Source code" archive) and extract it to a **writable** folder, e.g. `Desktop\TriAevum`.
   Don't use `Program Files`. Everything ends up in a folder called `Windows`.
3. Download **`topscreen211.zip`** (TopScreen 2.1.1). **Do not extract it.** Copy the ZIP as-is into
   that `Windows` folder, next to `TriAevumForge.exe`.
   Forge in this version can no longer download the TopScreen package by itself, so it must be there locally.
4. Run **`TriAevumForge.exe`** and select your decrypted ROM. Setup takes a few minutes; let it finish.
5. Press **Launch game**. From now on, start the game with **`TriAevum.exe`**.

```
Windows\
├── TriAevumForge.exe
├── TriAevum.exe
├── topscreen211.zip      ← put it here, still zipped
├── data\                 ← saves + settings live here, back this up
└── ...
```

## In-game

| Key | Action |
|---|---|
| **F1** | Settings (graphics, audio, controls, saves) |
| **F2** | Temporarily toggle the extra graphics effects |

Fullscreen: **F1 → Window mode → Borderless** (recommended) or *ExclusiveFullscreen*.

### Recommended settings for weaker AMD cards (RX 470 / RX 570 / RX 5500)

- **F1 → Preset → `Authentic`** (lightest), or lower/turn off **shadows**, **ambient occlusion (AO)** and **grass**.
- **FPS mode → `Original30`**.
- If it runs smoothly, raise the settings step by step.

### Controllers (PS4 / PS5 / Xbox)

- Connect the controller **before** starting `TriAevum.exe`. It's detected at launch.
- Close **Steam** (also from the tray) and tools like **DS4Windows**, since they can take over the controller.
- Check bindings under **F1 → Controls → Devices / Bindings**. The controller should be the active
  device, with its buttons assigned.
- Windows doesn't see the controller? Open *Set up USB game controllers*. Use a data cable (not a
  charge-only one), or pair over Bluetooth (hold **PS + Share** until the light flashes).
- **Sound comes out of the PS4 controller?** Over USB, Windows also adds the controller as an audio
  output. Switch the output back to your headset/speakers from the taskbar volume menu, then restart the game.
  To stop it for good, go to *Sound settings → Output → Wireless Controller → Don't allow*.
  This doesn't happen over Bluetooth.

## Why not the latest version?

The newest TriAevum releases (renamed **"Echoes of Time"**, v0.8.0-beta.1; also alpha.3/alpha.3b)
**do not work on AMD** in our testing:

- The game crashes a few seconds after launch, **inside the AMD Vulkan driver** (`amdvlk64.dll`,
  access violation `0xc0000005`), or stops with `vkQueueSubmit(frame) failed with VkResult -4`
  (`VK_ERROR_DEVICE_LOST`, which means the GPU driver reset). Tested on a Radeon RX 5500 XT with an up-to-date driver.
- The upstream README itself says **alpha.3 and alpha.3b are broken and withdrawn**, and the
  v0.8.0-beta.1 release notes say the first reports already include an **AMD driver crash**.
- The same crash is reported upstream on RX 6600 / 6650 / 6750 XT
  ([#126](https://github.com/coccofresco/TriAevum/issues/126),
  [#84](https://github.com/coccofresco/TriAevum/issues/84),
  [#118](https://github.com/coccofresco/TriAevum/issues/118)). In #84 a user also confirms that
  v0.6.0-alpha.2b runs on the same AMD PC.
- Workarounds we tried on v0.8.0-beta.1, none of which worked:
  - `TRIAEVUM_NRI_PIPELINE_LIBRARIES=0` removes the `amdvlk64.dll` crash, but you then get `VkResult -4` on the first frame.
  - Turning off shadows, AO and point shadows: same `VkResult -4`.
  - Deleting the renderer pipeline cache: same error.
  - The `Authentic` preset with `Original30` FPS: it either crashes immediately or loses the device.

### Why TopScreen has to be provided manually

v0.6.0-alpha.2b's Forge requires exactly **TopScreen 2.1.1** (`topscreen211.zip`,
179,430,508 bytes, SHA-256 below) and rejects anything else. That file has been removed from
GameBanana (only the newer `singlescreen_322.zip` is listed now), so Forge's automatic download gets
a ~49 KB web page instead of the archive and setup fails. Follow
[TRIAEVUM_TOPSCREEN_INSTALLATION.md](https://github.com/coccofresco/TriAevum/blob/main/docs/TRIAEVUM_TOPSCREEN_INSTALLATION.md),
or use the Drive mirror above, then put the ZIP next to `TriAevumForge.exe`. Forge then uses the
local file and doesn't try to download it.

Nothing in TriAevum itself is patched or modified. Both files are the original, unmodified
downloads, and you can check them against the hashes below.

**v0.6.0-alpha.2b (the official release ZIP) + TopScreen 2.1.1 provided locally** is the combination that installs cleanly and
runs on AMD. If a later release fixes AMD, this guide will be updated.

## Troubleshooting

| Problem | Fix |
|---|---|
| "TopScreen textures" error in Forge | `topscreen211.zip` isn't next to `TriAevumForge.exe`, or it was extracted. Redo step 3. |
| ROM rejected | It's encrypted, a `.cia`, or not USA/EUR. Use a decrypted `.3ds`/`.cci` dump. |
| Game opens then closes immediately | Update the GPU driver, use the `Authentic` preset, and check `Windows\logs\*.log`. |
| `VkResult -4` / crash in `amdvlk64.dll` | You're on a newer TriAevum build. Use v0.6.0-alpha.2b. |
| Lost saves | Never delete `data\`. Saves and settings are stored there. Back it up before updating. |

When reporting issues upstream, read the logs for personal info first and **never upload ROMs or game files**.

## Verifying downloads (optional)

SHA-256 of the files used for this guide:

```
a30730b98827e316743cdbed25e7d2f5c8343b44eeffe21b5249fa14c1a2246d  TriAevum-v0.6.0-alpha.2b-Windows-x64.zip
e0c143c872ccf4ad72033caab768753b147a4033d260a701204a63ee6bc16df4  topscreen211.zip
```

```powershell
Get-FileHash .\topscreen211.zip -Algorithm SHA256
```

## The ROM

You need a decrypted dump of **your own** Ocarina of Time 3D cartridge. With a homebrewed 3DS you
can dump it with [GodMode9](https://github.com/d0k3/GodMode9) (*Gamecart → Dump → `.3ds` decrypted*).
See [3ds.hacks.guide](https://3ds.hacks.guide/) to get started. This repo does not link to ROM
download sites.

## Credits

- **[TriAevum](https://github.com/coccofresco/TriAevum)** by coccofresco (GPL-3.0-or-later)
- **[TopScreen](https://gamebanana.com/mods/695893)** by **rlgcarrot**. Consider [buying them a coffee](https://ko-fi.com/rlgcarrot).
- Azahar, Ship of Harkinian, libultraship, NVIDIA NRI, SDL GameControllerDB
- **Nintendo** and **Grezzo** for the original game

## License

The text of this guide is under the [MIT License](LICENSE). That covers this repository's
documentation only. TriAevum, TopScreen and the game keep their own licenses and copyrights. This
project is not affiliated with Nintendo, Grezzo, TriAevum or TopScreen.
