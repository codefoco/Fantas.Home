# Quick Start on Mac

## Setup development

Fantas will Visual Studio Mac 2022 or superior.

* The latest [Visual Studio Mac](https://visualstudio.microsoft.com/#vsmac-section) is recommended.
* Install [⬇ Fantas.Extension](https://github.com/codefoco/Fantas.Home/releases/download/v1.0/Fantas.Extension.mpack) for Visual Studio Mac.

* Go to `Visual Studio > Extensions...` Menu option.

![screenshot_vs](images/vsmac_extensions.png)

* Install from file... and select the `Fantas.Extension.mpack`.
## Create new project

After installing Visual Studio Mac extension, create a new project using File New Project:

![screenshot_vs](images/screenshot_vsmac.png)

The extension added options to create a new game for Desktop, macOS (native), Android, iOS and tvOS


## Or Using dotnet new command line

If you want to create new game using new .NET instead of the classic you can do from the command line:

* Install Fantas templates for `dotnet new`.

```
dotnet new install Fantas.Templates
```

This will install .NET Fantas templates, you can crate a new desktop game doing 

```
mkdir MyGame
cd MyGame
dotnet new fantas-desktop
```

This are the templates available:

```
--------------------------------------------  --------------  --------  ---------------------------
Fantas Android Game (.NET)                    fantas-android  [C#]      android/mobile/games
Fantas Desktop DirectX Game (.NET - Windows)  fantas-directx  [C#]      desktop/games/linux/windows
Fantas Desktop Game (.NET)                    fantas-desktop  [C#]      desktop/games/linux/windows
Fantas iOS Game (.NET)                        fantas-ios      [C#]      ios/games
Fantas macOS Game (.NET)                      fantas-mac      [C#]      macos/games
Fantas tvOS Game (.NET)                       fantas-tvos     [C#]      tvos/games
```