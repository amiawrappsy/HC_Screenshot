# HC_Screenshot

A screenshot plugin for **HoneyCome** and **DigitalCraft** (ILLGames), similar to the screenshot manager for Koikatsu. It renders screenshots at a higher resolution than your game window, with an optional transparent background, and doesn't resize the window.

> **AI disclosure:** This plugin was written with AI assistance (Claude Opus 5.5, via Claude Code). I directed the work and tested it in-game, but the code was AI-generated.

## Features

- **High-resolution renders** at any size up to your GPU's limit (usually 16384 px), regardless of window size.
- **Supersampling** (1–4×) for smooth, anti-aliased edges.
- **Transparent background:** the scene is rendered over black and over white, and the transparency of each pixel is worked out from the difference. This works even though the game's shaders don't write a usable transparency channel.
- **Keeps post-processing** (bloom, color grading and so on) in transparent shots. You can turn this off.
- **Hides UI** in renders.
- **Screen capture** of the window as it looks, UI included, optionally upscaled.
- **Blocks the game's built-in F11 screenshot** in both HoneyCome and DigitalCraft, so you don't get duplicate or black images. You can turn this off. Card photos in the maker are unaffected.

## Requirements

- HoneyCome with **BepInEx 6 (IL2CPP)**, for example from [HF Patch](https://github.com/ManlyMarco/HoneyCome-HF_Patch)
- **Configuration Manager** (included in HF Patch), which provides the hotkey support

## Installation

1. Download the latest `HC_Screenshot_vX.X.X.zip` from the [Releases](https://github.com/amiawrappsy/HC_Screenshot/releases) page.
2. Extract it into your HoneyCome game folder, the one containing `HoneyCome.exe`. The DLL will end up in:

```
HoneyCome\BepInEx\plugins\HC_Screenshot\HC_Screenshot.dll
```

## Usage

| Hotkey | Action |
|---|---|
| **F9** | Capture the screen as it looks, UI included |
| **F11** | Render a high-resolution screenshot, no UI |
| **Shift+F11** | Turn the transparent background on or off |

Screenshots are saved to `HoneyCome\UserData\cap` by default. This includes DigitalCraft's, since it shares the main game's `UserData` folder.

All settings, including hotkeys, resolution, supersampling, transparency, output folder and format (PNG or JPG), can be changed in-game with the **Configuration Manager (F1)** under "Screenshot Manager".

### Tips

- The transparent background only removes empty space. Anything the camera actually sees, such as a map, floor or 3D backdrop, still appears in the image, so hide it first.
- If semi-transparent edges like hair look wrong in transparent shots, turn off **Post-processing in transparent shots**.
- Large renders take a few seconds. At the default settings (3840×2160, 2× supersampling) it's about 4–6 seconds. Higher supersampling costs a lot more: 3× at 3840×2160 can take around 30 seconds and uses a lot of memory.

## Building

Requires the .NET 6 SDK.

1. The project references the game's own assemblies. Edit `GameDir` in `HC_Screenshot.csproj` to point to your HoneyCome folder. The game must have been launched with BepInEx at least once so the `BepInEx\HoneyCome\interop` folder exists.
2. Run:

   ```
   dotnet build -c Release
   ```

The build copies the DLL into the game's `BepInEx\plugins\HC_Screenshot` folder automatically. Close the game first.

## Compatibility

Tested in HoneyCome (character maker) and DigitalCraft.

**DigitalCraft doesn't load BepInEx?** On some installs, the `DigitalCraft` folder is missing the files that start BepInEx, so no plugins load there at all. This isn't specific to this plugin. To fix it, copy `winhttp.dll` and `doorstop_config.ini` from the HoneyCome folder into `HoneyCome\DigitalCraft\`. Then edit the copied `doorstop_config.ini` so these three lines point back to the main folder, using your own install path:

```ini
target_assembly = C:\path\to\HoneyCome\BepInEx\core\BepInEx.Unity.IL2CPP.dll
coreclr_path = C:\path\to\HoneyCome\dotnet\coreclr.dll
corlib_dir = C:\path\to\HoneyCome\dotnet
```
