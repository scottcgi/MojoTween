## v2.0.0

_`2026-01-13 UTC+8 22:10`_

#### Added

* Added `TweenAction` extension method based on `Scrollbar`'s `ActionValueTo`.
* Added `PlayAll` method for `TweenManager`.
* Added `IsCompleting` method and `Completing` state for `Tween`.

#### Changed

* Changed the parameter of the `SetLoops` method in `TweenExtensions` from `OnCompleteByLoops` to `OnCompleteAllLoops`.
* Changed the `out` parameter type in calling methods to `var`.

#### Fixed

* Fixed the `Play` method of `Tween` where a previous `Rewind` in the `State.Paused` state was not reversed to `Play`.
* Fixed the `Play` method of `Tween` where playback sequence errors or omissions occurred in some cases when in the `State.Stopped` state.
* Fixed the `Rewind` method of `Tween` where a previous `Play` in the `State.Paused` state was not reversed to `Rewind`.
* Fixed the `Rewind` method of `Tween` where playback sequence errors or omissions occurred in some cases when in the `State.Stopped` state.
* Fixed the `Restart` method of `Tween` where the target state reset by `Restart` could be overwritten by the last execution of `UpdateTargetValues` when called between `Tween.UpdateActions` and `TweenAction.UpdateTargetValues`.
* Fixed an error in the `Reverse` method of `Tween` when `waitIndex` was out of range. This could cause the `Play` or `Rewind` sequence to incorrectly duplicate the last `Action` when reaching it, if another `Action`'s time region overlapped and was shorter. It also fixed an incorrect `waitIndex` position after `Stop`.
* Fixed an error in the `Reverse` method of `Tween` where an `Action` that had completed but was still removed from the update list could be incorrectly re-added to the update list.
* Fixed an issue in `Tween` where executing the `Reverse` method within a callback function during its completion process could cause index overflow.

#### Optimized

* Optimized the implementation of the `Reverse` method and the update logic for the internal `TweenAction` list state based on the `Completing` state.
* Optimized the reusable state judgment strategy in `Tween`'s `Play`, `Rewind`, `Restart`, `GotoStart`, `GotoEnd`, and `Reverse` methods, reducing control call restrictions and refactoring logic for different states.
* Optimized `Tween`'s runtime information display interface, increasing time precision from one to two decimal places.
* Optimized `TweenManager`'s `PlayAll`, `RewindAll`, `RestartAll`, and `Reverse` methods by excluding `Tween` instances that have stopped or completed and are about to be recycled.
* Optimized `TweenManager`'s `PauseAll` and `TogglePauseAll` methods by excluding cases where pausing or resuming was not possible.
* Improved code comment quality and formatting alignment.

#### Breaking Changed

* Changed method names in `Tween` for clearer meaning.
  * `IsStoppedByPlay` changed to `IsStoppedDuringPlay`.
  * `IsStoppedByRewind` changed to `IsStoppedDuringRewind`.
  * `IsCompletedByPlay` changed to `IsCompletedAfterPlay`.
  * `IsCompletedByRewind` changed to `IsCompletedAfterRewind`.

* Changed method names in `TweenExtensions` for clearer meaning.
  * `SetOnStartByPlay` changed to `SetOnStartForPlay`.
  * `SetOnStartByRewind` changed to `SetOnStartForRewind`.
  * `SetOnStopByPlay` changed to `SetOnStopForPlay`.
  * `SetOnStopByRewind` changed to `SetOnStopForRewind`.
  * `SetOnCompleteByPlay` changed to `SetOnCompleteForPlay`.
  * `SetOnCompleteByRewind` changed to `SetOnCompleteForRewind`.
  

## v1.3.6

_`2025-05-22 UTC+8 16:16`_

#### Fixed

* Fixed `#region Action Size` functions cannot play correctly with `isRelative` set to True.

#### Optimized

* Improved code comments and formatting.
  

## v1.3.5

_`2025-04-19 UTC+8 23:02`_

#### Fixed

* Fixed consecutively `Append TweenAction` for the same object's state with `isRelative` set to True, then the next `TweenAction initialization` will occur before previous `TweenAction completion`. 
* Fixed the `Tools/MojoTween/Tweens Info` display error with zero duration `TweenAction`.
  

## v1.3.3

_`2025-04-17 UTC+8 15:05`_

#### Added

* Added the `(Local)MoveAndRotate` TweenActionAPIs — with `#region (Local) Action Move And Rotate`.
* Added the `CanvasRenderer` TweenActionAPIs — with `#region Action CanvasRenderer `.

#### Optimized

* Optimized the `TweeAction`.
* Optimized the `TweenActionExtensions`.
* Improved code comments and formatting.

#### Removed

* Removed the unnecessary redundant functions in `TweenActionExtensions`.
* Removed the unnecessary redundant functions in `ColorExtensions`.


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
