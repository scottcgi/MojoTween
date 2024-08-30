## v1.2.0

_`2024-08-30 UTC+8 17:41`_

* Added functions of `TransformExtensions` for directly `Set` the size of `RectTransform` — with `#region Size`.
* Added functions of `TweenActionAPIExtensions` for the size animation of `RectTransform` — with `#region Action Size`.
* Added `TestSize()` for the size animation of `RectTransform` in `MojoTweenUI` scene.
* Removed the unnecessary redundant function `void GetSizeDelta(in Vector2 size, out Vector2 sizeDelta)` in `TransformExtensions`.


## v1.1.6

_`2024-03-26 UTC+8 22:56`_

* Added Assembly level BurstCompile FloatMode = FloatMode.Fast to TweenActionValue and TweenEase.
* Fixed the word spelling error in the TweenEase code comment.


## v1.1.4

_`2023-09-08 UTC+8 13:56`_

* Fixed UI text error of AboutMojoTweenMenu.
* Improved code comments and formatting.


## v1.1.3

_`2023-06-26 UTC+8 15:13`_

* Added the `SetOnUpdate` callback for Tween.
* Added the `SetOnUpdate` callback for TweenAction.
* Added the `SetOnUpdate` test in TweenTransform.
* Removed and Renamed some internal code of TweenAction. 
* Fixed the warning for the implicit operator from `NativeList<T>` to `NativeArray<T>` in UnityCollections-2.1.4 and later.
* Optimized the `UpdateActions` of Tween.
* Improved code comments and formatting.


## v1.0

_`2023-01-19 UTC+8 19:31`_

* Initial Release.
