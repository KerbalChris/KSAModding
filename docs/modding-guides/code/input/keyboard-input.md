<sub><sup>Tested with: **KSA Version 2025.11.8.2847**</sup></sub>
# Getting keyboard input from KSA

## Overview
This section will cover how to get keyboard input when modding KSA.

## Prerequisites
This guide uses harmony to intercept the `Vehicle.OnKey` function calls so we will need that setup first.
This guide continues from [Getting started with harmony](../harmony.md)

for this guide you will need:
* A visual studio solution set up for StarMap modding and with HarmonyLib as a NuGet package
* The Patcher class from [the previous guid](../harmony.md)

## Step 1. Creating the onKey function
To do actions when a key is pressed we will be hooking into the `Vehicle.OnKey` function. So lets set that up

In KSA Key events are handled using a combination of Glfw and some extra wrappers. So we will need some extra dependencies.
Add the following dependencies to your visual studio project:

* Brutal.Glfw.dll (Used for most Glfw types)
* Planet.Render.Core.dll (Used for RenderCore.Input.GlfwKeyEvent)

Now lets write the key input code

1. Add these using directives to the top of your `Patcher.cs` file
```csharp
using Brutal.GlfwApi;
using KSA;
```
2. now inside the `Patcher` class add the Harmony Patch
```csharp
[HarmonyPatch(typeof(Vehicle), nameof(Vehicle.OnKey))]
[HarmonyPrefix]
public static bool onKey(Vehicle __instance, RenderCore.Input.GlfwKeyEvent keyEvent)
{
    Console.WriteLine("A key was pressed!");
    return true;
}
```
The `Patcher.OnKey` function will automatically be called every time a key event happens (pressed/released/...) in game. If you want to add extra logic besides the print it should go in there.

## Step 2. Detecting for specific keys

Now that we can detect any key event, We can start filtering for the specific event we want.
For this tutorial we will try to print something every time the space bar is pressed.

Inside OnKey:

1. They GlfwKeyEvent contains 2 parts so we will first split those into GlfwKey and GlfwKeyAction.
2. We want to get the space bar so we will use an `if` statement to check for `GlfwKey.Space`
3. We only want to do something when it is pressed (Key goes down) not when released or repeated. So we will check for `GlfwKeyAction.Press`

4. Replace the contents of `onKey` with the following code.

```csharp
GlfwKey key = keyEvent.Key;
GlfwKeyAction action = keyEvent.Action;

if (key == GlfwKey.Space)
{
    if (action == GlfwKeyAction.Press)
    {
        Console.WriteLine("Space Pressed");
    }
}
return true;
```

We return true at the end to tell Harmony that the original method should still run.

In a Harmony prefix, returning:

true → "continue and execute the original Vehicle.OnKey"

false → "skip the original method completely"

Since this patch only wants to log the space key and not block or replace the game’s input handling, it finishes by returning true, allowing the game’s normal key-handling behavior to proceed.

## Step 3. Use the function
That is it you now have a functioning onKey function. If you want to add custom behavior just make sure you do it inside the `onKey` function and all will go well
