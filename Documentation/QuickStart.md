## Step 1. Install the dependent packages

Install the `Burst`, `Collections` and `Mathematics` from the Unity `Package Manager` by doing the following:

  * Open the menu `Window -> Package Manager`.
  * Click the first tab and Switch to `Packages: Unity Registry`.
  * Find `Burst`, `Collections` and `Mathematics` and install them one by one.
  * Restart the `Unity Editor`.

## Step 2. Set the script options

Open the script options by `[Edit] -> [Project Settting] -> [Player] -> [Other Settings]` and set two items:

 * First, add the `ENABLE_BURST_AOT` to `[Script Compilation] -> [Script Define Symbols]`.
 * Second, enable the `Allow 'unsafe' Code`.

Tip: **Any platform to build requires the above options to be set.**

## Step 3. Understand the package structure

* `MojoTween`
  * `Samples`
      * `Resources`
      * `Scenes`
        * `MojoTweenTransform.unity` — **Transform creates various Tweens.**
        * `MojoTweenUI.unity` — **UI creates various Tweens.**
      * `Scripts` — **The source code of samples.**
  * `Scripts`
    * `Editor`
      * `BaseEitor` — **An extension of the UnityEditor for Editor Tools.**
      * `Menus` — **The Tween Editor Tools under the menu "Tools/MojoTween".**
      * `MojoTween.Editor.asmdef` — **Generates the MojoTween.Editor.dll file.**
    * `link.xml` — **Prevent Burst code stripping from managed assemblies.**
    * `Runtime` 
      * `AssemblyInfo.cs` — **Enables the Editor code to access the internal method of the Runtime code.**
      * `Modules` — **The source code of Tween.**
      * `MojoTween.asmdef` — **Generates the MojoTween.dll file.**
      * `Utils` — **The extensions of Transform and Color for Tween.**

## Step 4. Run the samples

* Open the scene of samples by `Assets/MojoTweeen/Samples/Scenes`.
* Set the `[Game Window]` to `1080x1920 Portrait` (or higher resolution) and enable the `VSync (Game view only)`.
* Click the `Play` button.

Tip: **Open the `Tweens Info` window by the menu `Tools/MojoTween` to view the runtime Tweens.**

## Step 5. Integrate the package into the project

See the usage in this file `Assets/MojoTween/Samples/Scripts/Loop.cs` — just need to do the following two function calls:

 * First, update all Tweens step per frame.
   ```C#
   private void Update()
   {
       TweenManager.Update();
   }  
   ```
   
 * Second, dispose all Tweens native data when app quit.
   ```C#
   private void OnApplicationQuit()
   {
       TweenManager.DisposeAllNativeData();
   }
   ```
Tip: **See the [Documentation](./Documentation.md) for detailed API usage.**
