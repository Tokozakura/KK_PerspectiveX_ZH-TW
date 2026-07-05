# KK_PerspectiveX

A first-person POV plugin for Koikatsu that doesn't clip through the body and doesn't make you dizzy.

Works in the main game (free roam), H scenes, and CharaStudio.

## Why another POV mod?

Existing POV plugins have two problems. Some place the camera at the wrong spot, giving you a chest-level view with the head clipping into the screen. RealPOV places it correctly but copies the head bone's full animation rotation, including roll, so the view tilts sideways and jerks around with every animation. PerspectiveX handles position and rotation separately:

| | PerspectiveX |
|---|---|
| **Viewpoint** | True eye level, at the midpoint of the actual eye bones |
| **Rotation** | Fully mouse-controlled, with the horizon always level. An optional "animation sway" setting lets a configurable amount of head motion through, with roll always removed |
| **Position** | Follows the head through an adjustable smoother, so animation jitter doesn't hit your eyes |
| **Clipping** | The POV character's head, hair and head accessories are hidden while active, and restored when you leave POV |

## Installation

1. You need BepInEx 5 (already included in BetterRepack).
2. Download `KK_PerspectiveX.dll` from [Releases](../../releases) and drop it into `BepInEx/plugins/`.
3. If you have RealPOV installed (BetterRepack includes it), disable it by renaming `RealPOV.Koikatu.dll` to `RealPOV.Koikatu.dll.disabled`, since both plugins use Backspace as the toggle key.

## Controls

All rebindable via ConfigurationManager (F1).

- **Backspace**: toggle POV. In Studio, select a character in the workspace first.
- **Left mouse (hold + drag)**: look around. The cursor stays visible and usable otherwise.
- **Ctrl+L**: hands-free FPS mouse look. The cursor is captured until you press it again.
- **Ctrl+Shift+Left / Ctrl+Shift+Right**: switch the POV to the previous/next character.
- **Scroll wheel**: adjust FOV while looking around.

Comfort settings (position smoothing, animation sway, FOV, offsets) are in the plugin settings, adjustable live while in POV.

## Building from source

The plugin targets .NET Framework 3.5 and builds with the regular .NET SDK on any OS.
Copy these reference DLLs from your game install into `lib/` first (they are copyrighted, so not included here):

- `BepInEx/core/`: `BepInEx.dll`, `0Harmony.dll`
- `Koikatu_Data/Managed/`: `Assembly-CSharp.dll`, `Assembly-CSharp-firstpass.dll`, `UnityEngine.dll`, `UnityEngine.UI.dll`

Then:

```sh
cd src/KK_PerspectiveX
dotnet build -c Release
```

Output: `src/KK_PerspectiveX/bin/Release/KK_PerspectiveX.dll`

## License

[MIT](LICENSE)
