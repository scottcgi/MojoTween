## Step 1. Install the dependent packages

Install the `Burst`, `Collections` and `Mathematics` from the Unity `Package Manager` by doing the following:

  * Open the menu `Window -> Package Manager`.
  * Click the first tab and Switch to `Packages: Unity Registry`.
  * Find `Burst`, `Collections` and `Mathematics` and install them one by one.
  * Restart the `Unity Editor`.

## Step 2. Set the script options

Open the script options by `[Edit] -> [Project Settting] -> [Player] -> [Other Settings]` and set two items:

 * First, add the `ENABLE_BURST_AOT` to `[Script Compilation] -> [Script Define Symbols]`.
 * Second, enable the `Allow ‘unsafe’ Code`.

Tip: **Any platform to build requires the above options to be set.**

## Step 3. Understand the package structure

* `MojoUnity`
  * `Samples`
    * `MojoUnityTween`
      * `Resources`
      * `Scenes`
        * `Transform.unity` — **Transform creates various Tweens.**
        * `UI.unity` — **UI creates various Tweens.**
      * `Scripts` — **The source code of samples.**
  * `Scripts`
    * `Editor`
      * `BaseEitor` — **An extension of the UnityEditor for Editor Tools.**
      * `Menus` — **The Tween Editor Tools under the menu "Tools/MojoUnity/Tween".**
      * `MojoUnity.Editor.asmdef` — **Generates the MojoUnity.Editor.dll file.**
    * `link.xml` — **Prevent Burst code stripping from managed assemblies.**
    * `Runtime` 
      * `AssemblyInfo.cs` — **Enables the Editor code to access the internal method of the Runtime code.**
      * `Modules` — **The source code of Tween.**
      * `MojoUnity.asmdef` — **Generates the MojoUnity.dll file.**
      * `Utils` — **The extensions of Transform and Color for Tween.**

## Step 4. 

