<sub><sup>Tested with: **KSA Version 2025.11.8.2847**</sup></sub>
# Getting started with Harmony

## Overview
This guide will cover how to setup harmony.

"Harmony gives you an elegant and high level way to alter the functionality in applications written in C#." [^1]

## Prerequisites
This guide continues from the [first steps guide](first-steps.md)

for this guide you will need:

* A visual studio solution set up for StarMap modding and with HarmonyLib as a NuGet package

## Step 1. Creating the patcher class
Harmony loads classes the same way StarMap does so we will create a second class to not confuse the 2.

This class will be called `Patcher` and it will use the `HarmonyPatch` attribute.

1. Create a new file called Patcher.cs.
2. Add `using HarmonyLib` to the top of your file
```csharp
using HarmonyLib
```

3. Inside of the namespace put the `Patcher` class.
Note: the `"SimpleMod"` should always match the name of your mod .dll file.


```csharp
[HarmonyPatch]
internal static class Patcher
{
    private static Harmony? m_harmony = new Harmony("SimpleMod");
}
```

Now we need to have methods that can initialize harmony when the mod is loaded.
Add the following methods to `Patcher`

```csharp
public static void patch()
{
    m_harmony?.PatchAll(typeof(Patcher).Assembly);
}

public static void unload()
{
    m_harmony?.UnpatchAll("SimpleMod");
    m_harmony = null;
}
```

## Step 2. Using the patcher class

The patcher class is now done and ready to use. So lets put it to work.
Go to your main mod class (In our case: `SimpleMod`) and add the following functions:

```csharp
[StarMapAllModsLoaded]
public void fullyLoaded()
{
    Patcher.patch();
}

[StarMapUnload]
public void unload()
{
    Patcher.unload();
}
```
`fullyLoaded` will be called when all other mods are loaded and will start the patcher class.
`unload` will be called when unloading the mod and will handle cleanup.

## Step 3. There is no step 3
That's it you are now setup with Harmony.

## References
[^1]: [GitHub](https://github.com/pardeike/Harmony)