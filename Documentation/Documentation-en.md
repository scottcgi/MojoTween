## Overview

`TweenManager` controls all running Tweens and updates all Steps every frame. `Tween` controls both queued and concurrent TweenActions. `TweenAction` controls a list of TweenActionValues that will ease concurrently. `TweenActionValue` controls the target values. 

`Tween` is mainly used to build an action timeline, and then fill the timeline with actions. `TweenAction` can be created manually, but in most cases, the engine has extended a large number of existing `Unity Objects`.

Such as `Transform` can directly call the method with the `Action` prefix to create an `TweenAction`, just like `ActionMoveX(float x, float duration)`.

Both `Tween` and `TweenAction` support chained calls to set properties, including `ease`, `isRelative`, `callback` and more.

The detailed relationships of engine objects  can be found in the [Code Architecture](./CodeArchitecture.png) diagram.


* [TweenManager](#tweenmanager)
* 


## TweenManager

<details><summary><code>bool IsAnyUpdating()</code></summary>
  
```
Is there any Tween updating?
```
</details>

<details><summary><code>void StopAll()</code></summary>

```
Stop all updating Tweens Playing or Rewinding.
If the Tween is recyclable then it will be recycled.
```
</details>

<details><summary><code>void RestartAll()</code></summary>

```
Restart all updating Tweens Playing or Rewinding.
```
</details>

