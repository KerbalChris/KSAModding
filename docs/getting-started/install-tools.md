# Installing Tools

This page describes the basic tools you'll likely want when creating mods for KSA.

## Essentials

- A **text editor** (VS Code, Sublime Text, etc.) for editing config files and scripts.
- A **file archiver/browser** for inspecting game data if needed.
- The **KSA game** itself, installed in a location where you can access its files.

## StarMap (KSA Code Loader)

-   Download and unzip release from [Releases](https://github.com/StarMapLoader/StarMap/releases/latest).
-   Run StarMap.exe, this will fail and create a StarMapConfig.json.
-   Open StarMapConfig.json and set the location of your KSA installation.
    -   `GameLocation` should be set to the location where Kitten Space Agency was installed, pointing directly to that folder (e.g. `C:\\games\\Kitten Space Agency\\`)
    -   `RepositoryLocation` can be kept empty
-   Run StarMap.exe again, this should launch KSA and load your mods.

## Blender

1. Go to the [offical blender download website](https://www.blender.org/download/).
2. Download the installer for your OS (Windows, macOS, Linux)
3. Run the installer:
   - Windows: double-click .exe → follow prompts
   - macOS: drag Blender.app to Applications
   - Linux: extract tar.bz2 or use package manager
4. Open Blender
