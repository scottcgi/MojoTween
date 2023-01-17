## Step 1. Install dependent packages

Install the `Burst`, `Collections` and `Mathematics` from the Unity `Package Manager` by doing the following:

  * Open the menu `Window -> Package Manager`.
  * Click the first tab and Switch to `Packages: Unity Registry`.
  * Find `Burst`, `Collections` and `Mathematics` and install them one by one.
  * Restart the `Unity Editor`.

## Step 2. Set script options

Open the script options by `[Edit] -> [Project Settting] -> [Player] -> [Other Settings]` and set two items:

 * First, add the `ENABLE_BURST_AOT` to `[Script Compilation] -> [Script Define Symbols]`.
 * Second, enable the `Allow ‘unsafe’ Code`.

Tip: Any platform to be built needs to be set.

## Step 3

