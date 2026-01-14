## Overview v2.0.0

`TweenManager` – Controls **state switching** and **step updating** for all `Tween` instances.

`Tween` – Controls the **state** and **updates** of all its `TweenAction`s. It supports both **sequential** and **concurrent** execution modes. For example: Methods with the `Append` prefix add **queue actions**, while those with the `Add` prefix add **concurrent actions**.

`Tween` constructs a sequence timeline and fills it with various types of `TweenAction`s to achieve the orderly execution of **queue actions** and **concurrent actions**.

`TweenExtensions` – Extends the functionality of `Tween`, demonstrating how to use existing interfaces to customize `Tween`'s implementation.

`TweenAction` – Controls a group of `TweenActionValue`s. These **action values** execute concurrently to drive the progressive realization of a complete action.

`TweenActionExtensions` – Extends the functionality of `TweenAction`. Since `TweenAction` needs to be bound to a `Tween` to execute, this extension provides a default binding to a `Tween`, allowing `TweenAction` to be executed directly.

`TweenActionAPIExtensions` – Extends the functionality of `Unity Objects`, enabling them to directly create and use `TweenAction`s.

`TweenAction` can be created manually, but `TweenActionAPIExtensions` provides extensive property extensions for many `Unity Objects`, so typically manual creation is not required.

For example, for a `Transform`, you can directly call a series of methods prefixed with `Action` to create a `TweenAction`, similar to:

```C#
transform.ActionMoveX(float x, float duration)
```

`TweenActionValue` – Controls the **initialization**, **start**, and **end** of an object's property value, using **easing functions** to drive the value change.

`TweenEase` – Controls the routing and implementation of **easing functions**.

`Tween` and `TweenAction`, along with their extensions, all support **chained calls** to set various properties, such as: `ease`, `isRelative`, `callback`, etc.

The detailed structural relationships of the core `MojoTween` classes can be viewed in the [Code Architecture](./CodeArchitecture.png).

## Feature List

Below are the **methods** and **functions** of the core classes. Expanding each section reveals **usage instructions** and **examples**.

* [Tween](#tween)
  * [Create Tween](#create-tween)
  * [Append](#append)
  * [Add](#add)
  * [Add Delay](#add-delay)
  * [Add After](#add-after)
  * [Add Callback](#add-callback)
  * [Add Delay Callback](#add-delay-callback)
  * [Tween On Callback](#tween-on-callback)
  * [Set Default Properties](#set-default-properties)
  * [Controls](#controls)
  * [Test States](#test-states)
* [TweenExtensions](#tweenextensions)
* [TweenAction](#tweenaction)
  * [Create Action](#create-action)
  * [Action On Callback](#action-on-callback)
  * [Set Properties](#set-properties)
* [TweenActionExtensions](#tweenactionextensions)
* [TweenActionAPIExtensions](#tweenactionapiextensions)
  * [Transform Move](#transform-move)
  * [Transform Local Move](#transform-local-move)
  * [Transform Scale](#transform-scale)
  * [Transform Rotate](#transform-rotate)
  * [Transform Local Rotate](#transform-local-rotate)
  * [Transform Rotate And Move](#transform-rotate-and-move)
  * [Transform Local Rotate And Move](#transform-local-rotate-and-move)
  * [Transform Shake Position](#transform-shake-position)
  * [Transform Shake Scale](#transform-shake-scale)
  * [Transform Shake Rotation](#transform-shake-rotation)
  * [Transform Bezier Quadratic Move](#transform-bezier-quadratic-move)
  * [Transform Bezier Quadratic Local Move](#transform-bezier-quadratic-local-move)
  * [Transform Bezier Cubic Move](#transform-bezier-cubic-move)
  * [Transform Bezier Cubic Local Move](#transform-bezier-cubic-local-move)
  * [RectTransform Move](#recttransform-move)
  * [RectTransform OffsetMax](#recttransform-offsetmax)
  * [RectTransform OffsetMin](#recttransform-offsetmin)
  * [RectTransform SizeDelta](#recttransform-sizedelta)
  * [RectTransform Size](#recttransform-size)
  * [Graphic Color](#graphic-color)
  * [CanvasGroup Color](#canvasgroup-color)
  * [CanvasRenderer Color](#canvasrenderer-color)
  * [SpriteRenderer Color](#spriterenderer-color)
  * [AudioSource Volume](#audiosource-volume)
  * [Material Values](#material-values)
  * [Scrollbar Value](#scrollbar-value)
* [TweenManager](#tweenmanager)

## Tween

#### Create Tween

* <details>
  <summary>
    <code>static Tween Create(bool isRecyclable = true)</code>
  </summary>

  > Creates a Tween.
  >
  > If `[isRecyclable]` is `true`, the `Tween` will be automatically recycled upon completion — in this case, you don't need to keep a reference to a `Tween` for reuse, but can always create a new one.
  >
  > If `[isRecyclable]` is `false`, the `Tween` requires calling `SetRecyclable` for manual recycling — in this case, the `Tween` can use `Restart` and `Rewind`.

  ```C#
  // Will be automatically recycled
  var tween = Tween.Create();
  // Requires manual recycling
  var tween = Tween.Create(false);
  // Manual recycling method
  tween.SetRecyclable(true);
  ```
</details>

* <details>
  <summary>
    <code>static Tween PlayDelayCallback(float delay, Action Callback)</code>
  </summary>

  > Creates a `Tween` that executes a delayed callback function.

  ```C#
  Tween.PlayDelayCallback(1.0f, MyDelayCallbackAction);
  ```
</details>

#### Append

* <details>
  <summary>
    <code>Tween Append(TweenAction action)</code>
  </summary>

  > Appends a `TweenAction` to the action queue. The `Tween` will execute queue actions one after another.

  ```C#
  Tween.Create()
       .Append(MyQueueTweenAction1)
       .Append(MyQueueTweenAction2)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AppendInterval(float interval)</code>
  </summary>

  > Appends a time interval to the action queue, creating a delay between two actions in the `Tween` queue.

  ```C#
  Tween.Create()
       .AppendInterval(0.8f)
       .Append(MyQueueTweenAction1)
       .AppendInterval(1.5f)
       .Append(MyQueueTweenAction2)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AppendCallback(Action Callback)</code>
  </summary>

  > Appends a callback function to the action queue. When execution reaches this point in the queue, the callback function will be executed.

  ```C#
  Tween.Create()
       .AppendInterval(0.8f)
       .AppendCallback(MyQueueCallbackAction1)
       .AppendInterval(1.5f)
       .AppendCallback(MyQueueCallbackAction2)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AppendIntervalCallback(float interval, Action Callback)</code>
  </summary>

  > Appends a callback function with a time interval to the action queue. The queue will execute the callback after the specified delay.

  ```C#
  Tween.Create()
       .Append(MyQueueTweenAction1)
       .AppendIntervalCallback(0.7f, MyQueueCallbackAction2)
       .Play();
  ```
</details>

#### Add

* <details>
  <summary>
    <code>Tween Add(TweenAction action)</code>
  </summary>

  > Adds a `TweenAction` to the concurrent actions of the `Tween`. These actions will execute simultaneously.

  ```C#
  Tween.Create()
       .Add(MyConcurrentTweenAction1)
       .Add(MyConcurrentTweenAction2)
       .Play();
  ```
</details>

#### Add Delay

* <details>
  <summary>
    <code>Tween AddDelay(float delay, TweenAction action)</code>
  </summary>

  > Adds a delayed `TweenAction` to the concurrent actions. This action will start after a specified delay.

  ```C#
  Tween.Create()
       .AddDelay(0.0f, MyConcurrentTweenAction1)
       .AddDelay(0.2f, MyConcurrentTweenAction2)
       .AddDelay(0.4f, MyConcurrentTweenAction3)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddDelayAfterAppend(float delay, TweenAction action)</code>
  </summary>

  > Adds a delayed `TweenAction` to the concurrent actions. Its execution order is after the last `Append` action.

  ```C#
  Tween.Create()
       .Append(MyQueueTweenAction1)
       .AddDelayAfterAppend(0.2f, MyConcurrentTweenAction1)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddDelayAfterAdd(float delay, TweenAction action)</code>
  </summary>

  > Adds a delayed `TweenAction` to the concurrent actions. Its execution order is after the last `Add` action.

  ```C#
  Tween.Create()
       .Add(MyConcurrentTweenAction1)
       .AddDelayAfterAdd(0.2f, MyConcurrentTweenAction2)
       .Play();
  ```
</details>

#### Add After

* <details>
  <summary>
    <code>Tween AddAfterAppend(TweenAction action)</code>
  </summary>

  > Adds a `TweenAction` to the concurrent actions. Its execution order is after the last `Append` action.

  ```C#
  Tween.Create()
       .Append(MyQueueTweenAction1)
       .AddAfterAppend(MyConcurrentTweenAction1)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddAfterAdd(TweenAction action)</code>
  </summary>

  > Adds a `TweenAction` to the concurrent actions. Its execution order is after the last `Add` action.

  ```C#
  Tween.Create()
       .Add(MyConcurrentTweenAction1)
       .AddAfterAdd(MyConcurrentTweenAction2)
       .AddAfterAdd(MyConcurrentTweenAction3)
       .Play();
  ```
</details>

#### Add Callback

* <details>
  <summary>
    <code>Tween AddCallback(Action Callback)</code>
  </summary>

  > Adds a callback function to the concurrent actions.

  ```C#
  Tween.Create()
       .AddCallback(MyConcurrentCallbackAction1)
       .AddCallback(MyConcurrentCallbackAction2)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddCallbackAfterAppend(Action Callback)</code>
  </summary>

  > Adds a callback function to the concurrent actions. Its execution order is after the last `Append` action.

  ```C#
  Tween.Create()
       .AppendCallback(MyQueueCallbackAction1)
       .AddCallbackAfterAppend(MyConcurrentCallbackAction1)
       .AppendCallback(MyQueueCallbackAction2)
       .AddCallbackAfterAppend(MyConcurrentCallbackAction2)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddCallbackAfterAdd(Action Callback)</code>
  </summary>

  > Adds a callback function to the concurrent actions. Its execution order is after the last `Add` action.

  ```C#
  Tween.Create()
       .AddCallback(MyConcurrentCallbackAction1)
       .AddCallbackAfterAdd(MyConcurrentCallbackAction2)
       .AddCallbackAfterAdd(MyConcurrentCallbackAction3)
       .Play();
  ```
</details>

#### Add Delay Callback

* <details>
  <summary>
    <code>Tween AddDelayCallback(float delay, Action Callback)</code>
  </summary>

  > Adds a delayed callback function to the concurrent actions.

  ```C#
  Tween.Create()
       .AddDelayCallback(1.0f, MyConcurrentCallbackAction1)
       .AddDelayCallback(2.8f, MyConcurrentCallbackAction2)
       .AddDelayCallback(3.0f, MyConcurrentCallbackAction3)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddDelayCallbackAfterAppend(float delay, Action Callback)</code>
  </summary>

  > Adds a delayed callback function to the concurrent actions. Its execution order is after the last `Append` action.

  ```C#
  Tween.Create()
       .Append(MyQueueTweenAction1)
       .AddDelayCallbackAfterAppend(1.0f, MyConcurrentCallbackAction1)
       .Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween AddDelayCallbackAfterAdd(float delay, Action Callback)</code>
  </summary>

  > Adds a delayed callback function to the concurrent actions. Its execution order is after the last `Add` action.

  ```C#
  Tween.Create()
       .Add(MyConcurrentTweenAction1)
       .AddDelayCallbackAfterAdd(1.0f, MyConcurrentCallbackAction1)
       .Play();
  ```
</details>

#### Tween On Callback

* <details>
  <summary>
    <code>Tween SetOnStart(Action OnStart)</code>
  </summary>

  > Sets the callback function for when the `Tween` starts. Called when both playing and rewinding start.

  ```C#
  Tween.Create().SetOnStart(MyStartAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnUpdate(Action&lt;float&gt; OnUpdate)</code>
  </summary>

  > Sets the callback function for when the `Tween` updates. Called when both playing and rewinding update.
  > The callback parameter `float` ranges from `[0.0f, 1.0f]`, corresponding to the `Tween`'s progress from start to end.

  ```C#
  Tween.Create().SetOnUpdate(MyUpdateAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnComplete(Action OnComplete)</code>
  </summary>

  > Sets the callback function for when the `Tween` completes. Called when both playing and rewinding complete.

  ```C#
  Tween.Create().SetOnComplete(MyCompleteAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnStop(Action OnStop)</code>
  </summary>

  > Sets the callback function for when the `Tween` stops. Called when both playing and rewinding stop.

  ```C#
  Tween.Create().SetOnStop(MyStopAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnRecycle(Action OnRecycle)</code>
  </summary>

  > Sets the callback function for when the `Tween` is recycled.
  > You can use this function to clear data bound to the `Tween`.

  ```C#
  Tween.Create().SetOnRecycle(MyRecycleAction);
  ```
</details>

#### Set Default Properties

* <details>
  <summary>
    <code>Tween SetDefaultEase(TweenEase ease)</code>
  </summary>

  > Sets the default `[ease]` for `TweenAction`s when they are added. Default is `Smooth`.
  > Only `TweenAction`s that have not already set their `[ease]` (i.e., not `Smooth`) will use the `Tween`'s default.

  ```C#
  Tween.Create().SetDefaultEase(TweenEase.ExponentialOut);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetDefaultRelative(bool isRelative)</code>
  </summary>

  > Sets the default `[isRelative]` for `TweenAction`s when they are added. Default is `false`.
  > Only `TweenAction`s that have not already set their `[isRelative]` (i.e., not `false`) will use the `Tween`'s default.

  ```C#
  Tween.Create().SetDefaultRelative(true);
  ```
</details>

#### Controls

* <details>
  <summary>
    <code>Tween SetRecyclable(bool isRecyclable)</code>
  </summary>

  > Sets whether the `Tween` is recyclable.
  > If `true`, the `Tween` will be recycled directly when its state is `Setup`, `Stopped`, or `Completed`. Otherwise, it will be recycled after stopping or completion.
</details>

* <details>
  <summary>
    <code>Tween Play()</code>
  </summary>

  > Starts playing the `Tween`.
  > If the `Tween` is rewinding, or was previously rewinding, it will reverse to playing.
  > If the `Tween` has completed or stopped, it will restart playing.
</details>

* <details>
  <summary>
    <code>Tween Rewind()</code>
  </summary>

  > Starts rewinding the `Tween`.
  > If the `Tween` is playing, or was previously playing, it will reverse to rewinding.
  > If the `Tween` has completed or stopped, it will restart rewinding.
</details>

* <details>
  <summary>
    <code>Tween Restart()</code>
  </summary>

  > Restarts the `Tween`'s play or rewind.
  > If the `Tween` is playing, or was previously playing, it will restart playing.
  > If the `Tween` is rewinding, or was previously rewinding, it will restart rewinding.
  > If the `Tween` has completed, it will restart based on its previous state (play or rewind).
</details>

* <details>
  <summary>
    <code>Tween GotoStart()</code>
  </summary>

  > Directly jumps to the start of the `Tween`'s play or rewind.
  > The `Tween`'s state remains unchanged, only its position changes.
</details>

* <details>
  <summary>
    <code>Tween GotoEnd()</code>
  </summary>

  > Directly jumps to the end of the `Tween`'s play or rewind.
  > The `Tween` enters the completed state.
</details>

* <details>
  <summary>
    <code>Tween Reverse()</code>
  </summary>

  > Reverses the `Tween`'s running direction.
  > If playing, switches to rewind; if rewinding, switches to play; if not running, reverses the previous play or rewind direction.
</details>

* <details>
  <summary>
    <code>void Pause(bool isPause)</code>
  </summary>

  > Pauses or resumes a playing or rewinding `Tween`.
</details>

* <details>
  <summary>
    <code>Tween Stop()</code>
  </summary>

  > Stops the `Tween` from running.
  > If the `Tween` is recyclable, it will be recycled after stopping.
</details>

#### Test States

* <details>
  <summary>
    <code>bool IsSetup()</code>
  </summary>

  > Is the `Tween`'s state `Setup`?
</details>

* <details>
  <summary>
    <code>bool IsPlaying()</code>
  </summary>

  > Is the `Tween`'s state `Playing`?
</details>

* <details>
  <summary>
    <code>bool IsRewinding()</code>
  </summary>

  > Is the `Tween`'s state `Rewinding`?
</details>

* <details>
  <summary>
    <code>bool IsRunning()</code>
  </summary>

  > Is the `Tween`'s state `Playing` or `Rewinding`?
</details>

* <details>
  <summary>
    <code>bool IsPaused()</code>
  </summary>

  > Is the `Tween`'s state `Paused`?
</details>

* <details>
  <summary>
    <code>bool IsStopping()</code>
  </summary>

  > Is the `Tween`'s state `Stopping`?
</details>

* <details>
  <summary>
    <code>bool IsCompleting()</code>
  </summary>

  > Is the `Tween`'s state `Completing`?
</details>

* <details>
  <summary>
    <code>bool IsStopped()</code>
  </summary>

  > Is the `Tween`'s state `Stopped`?
</details>

* <details>
  <summary>
    <code>bool IsStoppedDuringPlay()</code>
  </summary>

  > Is the `Tween`'s state `Stopped` during play?
</details>

* <details>
  <summary>
    <code>bool IsStoppedDuringRewind()</code>
  </summary>

  > Is the `Tween`'s state `Stopped` during rewind?
</details>

* <details>
  <summary>
    <code>bool IsCompleted()</code>
  </summary>

  > Is the `Tween`'s state `Completed`?
</details>

* <details>
  <summary>
    <code>bool IsCompletedAfterPlay()</code>
  </summary>

  > Is the `Tween`'s state `Completed` after play?
</details>

* <details>
  <summary>
    <code>bool IsCompletedAfterRewind()</code>
  </summary>

  > Is the `Tween`'s state `Completed` after rewind?
</details>

* <details>
  <summary>
    <code>bool IsRecycled()</code>
  </summary>

  > Is the `Tween`'s state `Recycled`?
</details>

* <details>
  <summary>
    <code>bool IsOPRestart()</code>
  </summary>

  > Is the `Tween`'s operation state `Restart`?
  > This operation is instantaneous, involving only state switching and executing relevant callback functions. Therefore, you can use this check within those callbacks to determine the current operation state.
</details>

* <details>
  <summary>
    <code>bool IsOPGotoStart()</code>
  </summary>

  > Is the `Tween`'s operation state `GotoStart`?
  > This operation is instantaneous, involving only state switching and executing relevant callback functions. Therefore, you can use this check within those callbacks to determine the current operation state.
</details>

* <details>
  <summary>
    <code>bool IsOPGotoEnd()</code>
  </summary>

  > Is the `Tween`'s operation state `GotoEnd`?
  > This operation is instantaneous, involving only state switching and executing relevant callback functions. Therefore, you can use this check within those callbacks to determine the current operation state.
</details>

## TweenExtensions

* <details>
  <summary>
    <code>void TogglePause()</code>
  </summary>

  > Toggles the `Tween` between playing/rewinding and pausing.
</details>

* <details>
  <summary>
    <code>Tween SetOnStartForPlay(Action OnStartForPlay)</code>
  </summary>

  > Sets the callback function for when the `Tween` starts playing.
</details>

* <details>
  <summary>
    <code>Tween SetOnStartForRewind(Action OnStartForRewind)</code>
  </summary>

  > Sets the callback function for when the `Tween` starts rewinding.
</details>

* <details>
  <summary>
    <code>Tween SetOnStopForPlay(Action OnStopForPlay)</code>
  </summary>

  > Sets the callback function for when the `Tween` stops playing.
</details>

* <details>
  <summary>
    <code>Tween SetOnStopForRewind(Action OnStopForRewind)</code>
  </summary>

  > Sets the callback function for when the `Tween` stops rewinding.
</details>

* <details>
  <summary>
    <code>Tween SetOnCompleteForPlay(Action OnCompleteForPlay)</code>
  </summary>

  > Sets the callback function for when the `Tween` completes playing.
</details>

* <details>
  <summary>
    <code>Tween SetOnCompleteForRewind(Action OnCompleteForRewind)</code>
  </summary>

  > Sets the callback function for when the `Tween` completes rewinding.
</details>

* <details>
  <summary>
    <code>Tween SetLoops(int loops, bool isRewindToStart = false, Action OnCompleteAllLoops = null)</code>
  </summary>

  > Sets the number of times the `Tween` repeats playback.
  > Repeated calls will increase the playback and callback count.
  >
  > `[loops]`: The number of loops. `-1` means infinite loops.
  > `[isRewindToStart]`: When a loop completes, whether to rewind to the start before playing again. If `false`, it will `Restart` directly.
  > `[OnCompleteAllLoops]`: Callback function executed when all loops are complete.

  ```C#
  tween.SetLoops(1)
       .SetLoops(2, true, () => tween.Rewind().SetOnComplete(() => tween.SetRecyclable(true)))
       .Play();
  ```
</details>

## TweenAction

#### Create Action

* <details>
  <summary>
    <code>static TweenAction CreateFloat(Func&lt;float&gt; OnGetTargetFloat, Action&lt;float&gt; OnSetTargetFloat, float finalValue, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens a target object's property value to a `float` value.

  ```C#
  TweenAction.CreateFloat
  (
      ()      => transform.position.x,
      (value) => transform.SetPositionX(value),
      toFloatX,
      duration
  );
  ```
</details>

* <details>
  <summary>
    <code>static TweenAction CreateVector2(OnGetTargetVector2 OnGetTargetVector2, OnSetTargetVector2 OnSetTargetVector2, in Vector2 finalValues, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens a target object's property value to a `vector2` value.

  ```C#
  TweenAction.CreateVector2
  (
      (out Vector2 vector2) => vector2 = transform.position,
      (in  Vector2 vector2) => transform.SetPositionXY(vector2),
      toV2,
      duration
  );
  ```
</details>

* <details>
  <summary>
    <code>static TweenAction CreateVector3(OnGetTargetVector3 OnGetTargetVector3, OnSetTargetVector3 OnSetTargetVector3, in Vector3 finalValues, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens a target object's property value to a `vector3` value.

  ```C#
  TweenAction.CreateVector3
  (
      (out Vector3 vector3) => vector3 = transform.position,
      (in  Vector3 vector3) => transform.position = vector3,
      toV3,
      duration
  );
  ```
</details>

* <details>
  <summary>
    <code>static TweenAction CreateVector4(OnGetTargetVector4 OnGetTargetVector4, OnSetTargetVector4 OnSetTargetVector4, in Vector4 finalValues, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens a target object's property value to a `vector4` value.

  ```C#
  TweenAction.CreateVector4
  (
      (out Vector4 vector4) => vector4       = graphic.color,
      (in  Vector4 vector4) => graphic.color = vector4,
      toColor,
      duration
  );
  ```
</details>

#### Action On Callback

* <details>
  <summary>
    <code>TweenAction SetOnStart(Action OnStart)</code>
  </summary>

  > Sets the callback function for when the `TweenAction` starts. Called when both playing and rewinding start.
</details>

* <details>
  <summary>
    <code>TweenAction SetOnUpdate(Action&lt;float&gt; OnUpdate)</code>
  </summary>

  > Sets the callback function for when the `TweenAction` updates. Called when both playing and rewinding update.
  > The callback parameter `float` ranges from `[0.0f, 1.0f]`, corresponding to the `TweenAction`'s progress from start to end.
</details>

* <details>
  <summary>
    <code>TweenAction SetOnComplete(Action OnComplete)</code>
  </summary>

  > Sets the callback function for when the `TweenAction` completes. Called when both playing and rewinding complete.
</details>

#### Set Properties

* <details>
  <summary>
    <code>TweenAction SetRelative(bool isRelative)</code>
  </summary>

  > Sets the `[isRelative]` for all `TweenActionValue`s. Default is `false`.
</details>

* <details>
  <summary>
    <code>TweenAction SetRelativeAt(int index, bool isRelative)</code>
  </summary>

  > Sets the `[isRelative]` for the `TweenActionValue` at position `[index]`. Default is `false`.
  >
  > The prefix methods `Vector2/Vector3/Vector4` for creating `TweenAction` correspond to containing `2/3/4` `TweenActionValue`s. This allows you to set properties for a specific `TweenActionValue`.
</details>

* <details>
  <summary>
    <code>TweenAction SetEase(TweenEase ease)</code>
  </summary>

  > Sets the `[ease]` for all `TweenActionValue`s. Default is `Smooth`.
</details>

* <details>
  <summary>
    <code>TweenAction SetEaseAt(int index, TweenEase ease)</code>
  </summary>

  > Sets the `[ease]` for the `TweenActionValue` at position `[index]`. Default is `Smooth`.
  >
  > The prefix methods `Vector2/Vector3/Vector4` for creating `TweenAction` correspond to containing `2/3/4` `TweenActionValue`s. This allows you to set properties for a specific `TweenActionValue`.
</details>

* <details>
  <summary>
    <code>TweenAction SetExtraParams(params float[] extraParams)</code>
  </summary>

  > Sets a group of extra parameters `[extraParams]` for `TweenEase` to use.

  ```C#
  TweenAction.CreateFloat(...)
             .SetRelative(true)
             .SetEase(TweenEase.ShakeX)
             .SetExtraParams(amplitude, speed);
  ```
</details>

## TweenActionExtensions

* <details>
  <summary>
    <code>Tween Play()</code>
  </summary>

  > Plays the `TweenAction`. Creates a `Tween` to include the current action and returns this `Tween`.

  ```C#
  audioSource.ActionVolumeTo(toValue, duration).Play();
  ```
</details>

* <details>
  <summary>
    <code>Tween PlayDelay(float delay)</code>
  </summary>

  > Plays a delayed `TweenAction`. Creates a `Tween` to include the current action and returns this `Tween`.
</details>

## TweenActionAPIExtensions

**Note: Overloads of the same method are combined into one, using `/` to separate overload parameters, and `[]` to wrap overloadable parameters. That is, `[]` contains parameters for different overloads separated by `/`.**

#### Transform Move

* <details>
  <summary>
    <code>TweenAction ActionMoveX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position x` to `[x]`.
  > Creates a `TweenAction` that moves `Transform position y` to `[y]`.
  > Creates a `TweenAction` that moves `Transform position z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveXY([in Vector2 v2/float x, float y/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xy` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform position xy` to `[xy]`.
  > Creates a `TweenAction` that moves `Transform position xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveXZ([in Vector2 v2/float x, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xz` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform position xz` to `[xz]`.
  > Creates a `TweenAction` that moves `Transform position xz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveYZ([in Vector2 v2/float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position yz` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform position yz` to `[yz]`.
  > Creates a `TweenAction` that moves `Transform position yz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMove([in Vector3 v3/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position` to `[v3]`.
  > Creates a `TweenAction` that moves `Transform position` to `[xyz]`.
  > Creates a `TweenAction` that moves `Transform position` to `[final]`.
</details>

#### Transform Local Move

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition x` to `[x]`.
  > Creates a `TweenAction` that moves `Transform localPosition y` to `[y]`.
  > Creates a `TweenAction` that moves `Transform localPosition z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveXY([in Vector2 v2/float x, float y/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[xy]`.
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveXZ([in Vector2 v2/float x, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[xz]`.
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveYZ([in Vector2 v2/float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[v2]`.
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[yz]`.
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalMove([in Vector3 v3/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition` to `[v3]`.
  > Creates a `TweenAction` that moves `Transform localPosition` to `[xyz]`.
  > Creates a `TweenAction` that moves `Transform localPosition` to `[final]`.
</details>

#### Transform Scale

* <details>
  <summary>
    <code>TweenAction ActionScaleX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that scales `Transform localScale x` to `[x]`.
  > Creates a `TweenAction` that scales `Transform localScale y` to `[y]`.
  > Creates a `TweenAction` that scales `Transform localScale z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionScaleXY([in Vector2 v2/float x, float y/float value/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that scales `Transform localScale xy` to `[v2]`.
  > Creates a `TweenAction` that scales `Transform localScale xy` to `[xy]`.
  > Creates a `TweenAction` that scales `Transform localScale xy` to `[value]`.
  > Creates a `TweenAction` that scales `Transform localScale xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionScaleXZ([in Vector2 v2/float x, float z/float value/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that scales `Transform localScale xz` to `[v2]`.
  > Creates a `TweenAction` that scales `Transform localScale xz` to `[xz]`.
  > Creates a `TweenAction` that scales `Transform localScale xz` to `[value]`.
  > Creates a `TweenAction` that scales `Transform localScale xz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionScaleYZ([in Vector2 v2/float y, float z/float value/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that scales `Transform localScale yz` to `[v2]`.
  > Creates a `TweenAction` that scales `Transform localScale yz` to `[yz]`.
  > Creates a `TweenAction` that scales `Transform localScale yz` to `[value]`.
  > Creates a `TweenAction` that scales `Transform localScale yz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionScale([in Vector3 v3/float x, float y, float z/float value/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that scales `Transform localScale` to `[v3]`.
  > Creates a `TweenAction` that scales `Transform localScale` to `[xyz]`.
  > Creates a `TweenAction` that scales `Transform localScale` to `[value]`.
  > Creates a `TweenAction` that scales `Transform localScale` to `[final]`.
</details>

#### Transform Rotate

* <details>
  <summary>
    <code>TweenAction ActionRotateX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform eulerAngles x` to `[x]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles y` to `[y]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRotateXY([in Vector2 v2/float x, float y/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform eulerAngles xy` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles xy` to `[xy]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRotateXZ([in Vector2 v2/float x, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform eulerAngles xz` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles xz` to `[xz]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles xz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRotateYZ([in Vector2 v2/float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform eulerAngles yz` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles yz` to `[yz]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles yz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRotate([in Vector3 v3/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform eulerAngles` to `[v3]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles` to `[xyz]`.
  > Creates a `TweenAction` that rotates `Transform eulerAngles` to `[final]`.
</details>

#### Transform Local Rotate

* <details>
  <summary>
    <code>TweenAction ActionLocalRotateX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform localEulerAngles x` to `[x]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles y` to `[y]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalRotateXY([in Vector2 v2/float x, float y/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform localEulerAngles xy` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles xy` to `[xy]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalRotateXZ([in Vector2 v2/float x, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform localEulerAngles xz` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles xz` to `[xz]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles xz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalRotateYZ([in Vector2 v2/float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform localEulerAngles yz` to `[v2]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles yz` to `[yz]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles yz` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalRotate([in Vector3 v3/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that rotates `Transform localEulerAngles` to `[v3]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles` to `[xyz]`.
  > Creates a `TweenAction` that rotates `Transform localEulerAngles` to `[final]`.
</details>

#### Transform Rotate And Move

* <details>
  <summary>
    <code>TweenAction ActionMoveXYAndRotateZ([in Vector3 v3/in Vector2 v2, float z/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xy` to `[v3.xy]` and rotates `Transform eulerAngles z` to `[v3.z]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[v2]` and rotates `Transform eulerAngles z` to `[z]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[xy]` and rotates `Transform eulerAngles z` to `[z]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[final]` and rotates `Transform eulerAngles z` to `[final]`.
  >
  > Optimized using `Transform`'s built-in methods `GetPositionAndRotation` and `SetPositionAndRotation`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveAndRotate([in Vector3 position, in Vector3 eulerAngles/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position` to `[position]` and rotates `Transform eulerAngles` to `[eulerAngles]`.
  >
  > Creates a `TweenAction` that moves `Transform position` to `[final]` and rotates `Transform eulerAngles` to `[final]`.
  >
  > Optimized using `Transform`'s built-in methods `GetPositionAndRotation` and `SetPositionAndRotation`.
</details>

#### Transform Local Rotate And Move

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveXYAndRotateZ([in Vector3 v3/in Vector2 v2, float z/float x, float y, float z/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[v3.xy]` and rotates `Transform localEulerAngles z` to `[v3.z]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[v2]` and rotates `Transform localEulerAngles z` to `[z]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[xy]` and rotates `Transform localEulerAngles z` to `[z]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[final]` and rotates `Transform localEulerAngles z` to `[final]`.
  >
  > Optimized using `Transform`'s built-in methods `GetPositionAndRotation` and `SetPositionAndRotation`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionLocalMoveAndRotate([in Vector3 localPosition, in Vector3 localEulerAngles/Transform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition` to `[localPosition]` and rotates `Transform localEulerAngles` to `[localEulerAngles]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition` to `[final]` and rotates `Transform localEulerAngles` to `[final]`.
  >
  > Optimized using `Transform`'s built-in methods `GetPositionAndRotation` and `SetPositionAndRotation`.
</details>

#### Transform Shake Position

* <details>
  <summary>
    <code>TweenAction ActionShakePositionX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform position x` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform position y` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform position z` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakePositionXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform position xy` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform position xz` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform position yz` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakePosition(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform position` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

#### Transform Shake Scale

* <details>
  <summary>
    <code>TweenAction ActionShakeScaleX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform scale x` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform scale y` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform scale z` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakeScaleXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform scale xy` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform scale xz` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform scale yz` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakeScale(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform scale` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

#### Transform Shake Rotation

* <details>
  <summary>
    <code>TweenAction ActionShakeRotationX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform rotation x` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform rotation y` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform rotation z` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakeRotationXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform rotation xy` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform rotation xz` based on `[amplitude]` and `[speed]`.
  > Creates a `TweenAction` that shakes `Transform rotation yz` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

* <details>
  <summary>
    <code>TweenAction ActionShakeRotation(float amplitude, float speed, float duration)</code>
  </summary>

  > Creates a `TweenAction` that shakes `Transform rotation` based on `[amplitude]` and `[speed]`.
  >
  > Note: Do not modify the `isRelative` and `TweenEase` of the `TweenAction`, as the shake has fixed settings.
</details>

#### Transform Bezier Quadratic Move

* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveXY(
      [in Vector2 v2/float x, float y/Transform final],
      [in Vector2 controlPos/float controlPosX, float controlPosY/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xy` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[xy]` using a `Bezier2` control point `[controlPosXY]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2MoveXY(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2MoveXY(toX, toY,    controlPosX, controlPosY, duration);
  transform.ActionBezier2MoveXY(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveXZ(
      [in Vector2 v2/float x, float z/Transform final],
      [in Vector2 controlPos/float controlPosX, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xz` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position xz` to `[xz]` using a `Bezier2` control point `[controlPosXZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position xz` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2MoveXZ(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2MoveXZ(toX, toZ,    controlPosX, controlPosZ, duration);
  transform.ActionBezier2MoveXZ(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveYZ(
      [in Vector2 v2/float y, float z/Transform final],
      [in Vector2 controlPos/float controlPosY, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position yz` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position yz` to `[yz]` using a `Bezier2` control point `[controlPosYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position yz` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2MoveYZ(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2MoveYZ(toY, toZ,    controlPosY, controlPosZ, duration);
  transform.ActionBezier2MoveYZ(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2Move(
      [in Vector3 v3/float x, float y, float z/Transform final],
      [in Vector3 controlPos/float controlPosX, float controlPosY, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position` to `[v3]` using a `Bezier2` control point `[controlPosVector3]`.
  >
  > Creates a `TweenAction` that moves `Transform position` to `[xyz]` using a `Bezier2` control point `[controlPosXYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2Move(toVector3,     controlPosVector3,                     duration);
  transform.ActionBezier2Move(toX, toY, toZ, controlPosX, controlPosY, controlPosZ, duration);
  transform.ActionBezier2Move(toTransform,   controlPosTransform,                   duration);
  ```
</details>

#### Transform Bezier Quadratic Local Move

* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveXY(
      [in Vector2 v2/float x, float y/Transform final],
      [in Vector2 controlPos/float controlPosX, float controlPosY/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[xy]` using a `Bezier2` control point `[controlPosXY]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2LocalMoveXY(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2LocalMoveXY(toX, toY,    controlPosX, controlPosY, duration);
  transform.ActionBezier2LocalMoveXY(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveXZ(
      [in Vector2 v2/float x, float z/Transform final],
      [in Vector2 controlPos/float controlPosX, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[xz]` using a `Bezier2` control point `[controlPosXZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2LocalMoveXZ(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2LocalMoveXZ(toX, toZ,    controlPosX, controlPosZ, duration);
  transform.ActionBezier2LocalMoveXZ(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveYZ(
      [in Vector2 v2/float y, float z/Transform final],
      [in Vector2 controlPos/float controlPosY, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[v2]` using a `Bezier2` control point `[controlPosVector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[yz]` using a `Bezier2` control point `[controlPosYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2LocalMoveYZ(toVector2,   controlPosVector2,        duration);
  transform.ActionBezier2LocalMoveYZ(toY, toZ,    controlPosY, controlPosZ, duration);
  transform.ActionBezier2LocalMoveYZ(toTransform, controlPosTransform,      duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMove(
      [in Vector3 v3/float x, float y, float z/Transform final],
      [in Vector3 controlPos/float controlPosX, float controlPosY, float controlPosZ/Transform controlPos],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition` to `[v3]` using a `Bezier2` control point `[controlPosVector3]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition` to `[xyz]` using a `Bezier2` control point `[controlPosXYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition` to `[final]` using a `Bezier2` control point `[controlPosTransform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier2LocalMove(toVector3,     controlPosVector3,                     duration);
  transform.ActionBezier2LocalMove(toX, toY, toZ, controlPosX, controlPosY, controlPosZ, duration);
  transform.ActionBezier2LocalMove(toTransform,   controlPosTransform,                   duration);
  ```
</details>

#### Transform Bezier Cubic Move

* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveXY(
      [in Vector2 v2/float x, float y/Transform final],
      [in Vector2 controlPos1/float controlPos1X, float controlPos1Y/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2X, float controlPos2Y/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xy` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[xy]` using `Bezier3` control points `[controlPos1XY]` and `[controlPos2XY]`.
  >
  > Creates a `TweenAction` that moves `Transform position xy` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3MoveXY(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3MoveXY(toX, toY,    controlPos1X, controlPos1Y, controlPos2X, controlPos2Y, duration);
  transform.ActionBezier3MoveXY(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveXZ(
      [in Vector2 v2/float x, float z/Transform final],
      [in Vector2 controlPos1/float controlPos1X, float controlPos1Z/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2X, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position xz` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position xz` to `[xz]` using `Bezier3` control points `[controlPos1XZ]` and `[controlPos2XZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position xz` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3MoveXZ(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3MoveXZ(toX, toZ,    controlPos1X, controlPos1Z, controlPos2X, controlPos2Z, duration);
  transform.ActionBezier3MoveXZ(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveYZ(
      [in Vector2 v2/float y, float z/Transform final],
      [in Vector2 controlPos1/float controlPos1Y, float controlPos1Z/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2Y, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position yz` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform position yz` to `[yz]` using `Bezier3` control points `[controlPos1YZ]` and `[controlPos2YZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position yz` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3MoveYZ(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3MoveYZ(toY, toZ,    controlPos1Y, controlPos1Z, controlPos2Y, controlPos2Z, duration);
  transform.ActionBezier3MoveYZ(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3Move(
      [in Vector3 v3/float x, float y, float z/Transform final],
      [in Vector3 controlPos1/float controlPos1X, float controlPos1Y, float controlPos1Z/Transform controlPos1],
      [in Vector3 controlPos2/float controlPos2X, float controlPos2Y, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform position` to `[v3]` using `Bezier3` control points `[controlPos1Vector3]` and `[controlPos2Vector3]`.
  >
  > Creates a `TweenAction` that moves `Transform position` to `[xyz]` using `Bezier3` control points `[controlPos1XYZ]` and `[controlPos2XYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform position` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3Move(toVector3,     controlPos1Vector3,                       controlPos2Vector3,                       duration);
  transform.ActionBezier3Move(toX, toY, toZ, controlPos1X, controlPos1Y, controlPos1Z, controlPos2X, controlPos2Y, controlPos2Z, duration);
  transform.ActionBezier3Move(toTransform,   controlPos1Transform,                     controlPos2Transform,                     duration);
  ```
</details>

#### Transform Bezier Cubic Local Move

* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveXY(
      [in Vector2 v2/float x, float y/Transform final],
      [in Vector2 controlPos1/float controlPos1X, float controlPos1Y/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2X, float controlPos2Y/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[xy]` using `Bezier3` control points `[controlPos1XY]` and `[controlPos2XY]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xy` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3LocalMoveXY(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3LocalMoveXY(toX, toY,    controlPos1X, controlPos1Y, controlPos2X, controlPos2Y, duration);
  transform.ActionBezier3LocalMoveXY(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveXZ(
      [in Vector2 v2/float x, float z/Transform final],
      [in Vector2 controlPos1/float controlPos1X, float controlPos1Z/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2X, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[xz]` using `Bezier3` control points `[controlPos1XZ]` and `[controlPos2XZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition xz` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3LocalMoveXZ(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3LocalMoveXZ(toX, toZ,    controlPos1X, controlPos1Z, controlPos2X, controlPos2Z, duration);
  transform.ActionBezier3LocalMoveXZ(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveYZ(
      [in Vector2 v2/float y, float z/Transform final],
      [in Vector2 controlPos1/float controlPos1Y, float controlPos1Z/Transform controlPos1],
      [in Vector2 controlPos2/float controlPos2Y, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[v2]` using `Bezier3` control points `[controlPos1Vector2]` and `[controlPos2Vector2]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[yz]` using `Bezier3` control points `[controlPos1YZ]` and `[controlPos2YZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition yz` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3LocalMoveYZ(toVector2,   controlPos1Vector2,         controlPos2Vector2,         duration);
  transform.ActionBezier3LocalMoveYZ(toY, toZ,    controlPos1Y, controlPos1Z, controlPos2Y, controlPos2Z, duration);
  transform.ActionBezier3LocalMoveYZ(toTransform, controlPos1Transform,       controlPos2Transform,       duration);
  ```
</details>

* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMove(
      [in Vector3 v3/float x, float y, float z/Transform final],
      [in Vector3 controlPos1/float controlPos1X, float controlPos1Y, float controlPos1Z/Transform controlPos1],
      [in Vector3 controlPos2/float controlPos2X, float controlPos2Y, float controlPos2Z/Transform controlPos2],
      float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `Transform localPosition` to `[v3]` using `Bezier3` control points `[controlPos1Vector3]` and `[controlPos2Vector3]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition` to `[xyz]` using `Bezier3` control points `[controlPos1XYZ]` and `[controlPos2XYZ]`.
  >
  > Creates a `TweenAction` that moves `Transform localPosition` to `[final]` using `Bezier3` control points `[controlPos1Transform]` and `[controlPos2Transform]`.
  >
  > Note: Do not modify the `TweenEase` of the `TweenAction`, as the Bezier curve has fixed settings.

  ```C#
  transform.ActionBezier3LocalMove(toVector3,     controlPos1Vector3,                       controlPos2Vector3,                       duration);
  transform.ActionBezier3LocalMove(toX, toY, toZ, controlPos1X, controlPos1Y, controlPos1Z, controlPos2X, controlPos2Y, controlPos2Z, duration);
  transform.ActionBezier3LocalMove(toTransform,   controlPos1Transform,                     controlPos2Transform,                     duration);
  ```
</details>

#### RectTransform Move

* <details>
  <summary>
    <code>TweenAction ActionMoveAnchoredX/Y/Z([float x/float y/float z], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `RectTransform anchoredPosition x` to `[x]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition y` to `[y]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition3D z` to `[z]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveAnchoredXY([in Vector2 v2/float x, float y/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `RectTransform anchoredPosition xy` to `[v2]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition xy` to `[xy]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition xy` to `[final]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveAnchored([in Vector3 v3/float x, float y, float z/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that moves `RectTransform anchoredPosition3D` to `[v3]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition3D` to `[xyz]`.
  > Creates a `TweenAction` that moves `RectTransform anchoredPosition3D` to `[final]`.
</details>

#### RectTransform OffsetMax

* <details>
  <summary>
    <code>TweenAction ActionOffsetMaxX/Y([float x/float y], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform offsetMax x` to `[x]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMax y` to `[y]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionOffsetMax([in Vector2 v2/float x, float y/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform offsetMax` to `[v2]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMax` to `[xy]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMax` to `[final]`.
</details>

#### RectTransform OffsetMin

* <details>
  <summary>
    <code>TweenAction ActionOffsetMinX/Y([float x/float y], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform offsetMin x` to `[x]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMin y` to `[y]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionOffsetMin([in Vector2 v2/float x, float y/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform offsetMin` to `[v2]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMin` to `[xy]`.
  > Creates a `TweenAction` that changes `RectTransform offsetMin` to `[final]`.
</details>

#### RectTransform SizeDelta

* <details>
  <summary>
    <code>TweenAction ActionSizeDeltaX/Y([float x/float y], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform sizeDelta x` to `[x]`.
  > Creates a `TweenAction` that changes `RectTransform sizeDelta y` to `[y]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionSizeDelta([in Vector2 v2/float x, float y/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform sizeDelta` to `[v2]`.
  > Creates a `TweenAction` that changes `RectTransform sizeDelta` to `[xy]`.
  > Creates a `TweenAction` that changes `RectTransform sizeDelta` to `[final]`.
</details>

#### RectTransform Size

* <details>
  <summary>
    <code>TweenAction ActionSizeX/Y([float x/float y], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform size x` to `[x]`.
  > Creates a `TweenAction` that changes `RectTransform size y` to `[y]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionSize([in Vector2 v2/float x, float y/RectTransform final], float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `RectTransform size` to `[v2]`.
  > Creates a `TweenAction` that changes `RectTransform size` to `[xy]`.
  > Creates a `TweenAction` that changes `RectTransform size` to `[final]`.
</details>

#### Graphic Color

* <details>
  <summary>
    <code>TweenAction ActionFadeTo(float alpha, float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades `Graphic color alpha` to `[alpha]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionFadeIn/Out(float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades in `Graphic color alpha` to `[1.0f]`.
  > Creates a `TweenAction` that fades out `Graphic color alpha` to `[0.0f]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionColorTo(in Color color, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `Graphic color` to `[color]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRGBTo(in Color color/ in Vector3 rgb, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `Graphic color rgb` to `[color]`.
  > Creates a `TweenAction` that tweens `Graphic color rgb` to `[rgb]`.
</details>

#### CanvasGroup Color

* <details>
  <summary>
    <code>TweenAction ActionFadeTo(float alpha, float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades `CanvasGroup color alpha` to `[alpha]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionFadeIn/Out(float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades in `CanvasGroup color alpha` to `[1.0f]`.
  > Creates a `TweenAction` that fades out `CanvasGroup color alpha` to `[0.0f]`.
</details>

#### CanvasRenderer Color

* <details>
  <summary>
    <code>TweenAction ActionFadeTo(float alpha, float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades `CanvasRenderer color alpha` to `[alpha]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionFadeIn/Out(float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades in `CanvasRenderer color alpha` to `[1.0f]`.
  > Creates a `TweenAction` that fades out `CanvasRenderer color alpha` to `[0.0f]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionColorTo(in Color color, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `CanvasRenderer color` to `[color]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRGBTo(in Color color/in Vector3 rgb, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `CanvasRenderer color rgb` to `[color]`.
  > Creates a `TweenAction` that tweens `CanvasRenderer color rgb` to `[rgb]`.
</details>

#### SpriteRenderer Color

* <details>
  <summary>
    <code>TweenAction ActionFadeTo(float alpha, float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades `SpriteRenderer color alpha` to `[alpha]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionFadeIn/Out(float duration)</code>
  </summary>

  > Creates a `TweenAction` that fades in `SpriteRenderer color alpha` to `[1.0f]`.
  > Creates a `TweenAction` that fades out `SpriteRenderer color alpha` to `[0.0f]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionColorTo(in Color color, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `SpriteRenderer color` to `[color]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionRGBTo(in Color color/in Vector3 rgb, float duration)</code>
  </summary>

  > Creates a `TweenAction` that tweens `SpriteRenderer color rgb` to `[color]`.
  > Creates a `TweenAction` that tweens `SpriteRenderer color rgb` to `[rgb]`.
</details>

#### AudioSource Volume

* <details>
  <summary>
    <code>TweenAction ActionVolumeTo(float volume, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `AudioSource volume` to `[volume]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionVolumeIn/Out(float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `AudioSource volume` to `[1.0f]`.
  > Creates a `TweenAction` that changes `AudioSource volume` to `[0.0f]`.
</details>

#### Material Values

* <details>
  <summary>
    <code>TweenAction ActionFloatTo(string name, float value, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes the `float` property named `[name]` of a `Material` to `[value]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionIntTo(string name, int value, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes the `int` property named `[name]` of a `Material` to `[value]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionVectorTo(string name, in Vector4 v4, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes the `vector` property named `[name]` of a `Material` to `[v4]`.
</details>

* <details>
  <summary>
    <code>TweenAction ActionColorTo(string name, in Color color, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes the `color` property named `[name]` of a `Material` to `[color]`.
</details>

#### Scrollbar Value

* <details>
  <summary>
    <code>TweenAction ActionValueTo(float value, float duration)</code>
  </summary>

  > Creates a `TweenAction` that changes `Scrollbar value` to `[value]`.
</details>

## TweenManager

* <details>
  <summary>
    <code>static bool IsAnyUpdating()</code>
  </summary>

  > Is any `Tween` currently updating?
</details>

* <details>
  <summary>
    <code>static void PlayAll()</code>
  </summary>

  > Plays all updating `Tween`s, except those that have stopped or completed and are about to be automatically recycled.
  > Note: If a `Tween` is currently or was previously rewinding, it will be reversed.
</details>

* <details>
  <summary>
    <code>static void RewindAll()</code>
  </summary>

  > Rewinds all updating `Tween`s, except those that have stopped or completed and are about to be automatically recycled.
</details>

* <details>
  <summary>
    <code>static void RestartAll()</code>
  </summary>

  > Restarts all updating `Tween`s, except those that have stopped or completed and are about to be automatically recycled.
</details>

* <details>
  <summary>
    <code>static void ReverseAll()</code>
  </summary>

  > Reverses all updating `Tween`s, except those that have stopped or completed and are about to be automatically recycled.
</details>

* <details>
  <summary>
    <code>static void PauseAll(bool isPause)</code>
  </summary>

  > Pauses or resumes all updating `Tween`s.
</details>

* <details>
  <summary>
    <code>static void TogglePauseAll()</code>
  </summary>

  > Toggles pause/resume for all updating `Tween`s.
</details>

* <details>
  <summary>
    <code>static void StopAll()</code>
  </summary>

  > Stops all updating `Tween`s, except those in `Setup`, `Stopping`, `Stopped`, `Completing`, or `Completed` states.
  > Note: If a `Tween` is recyclable, it will be recycled after stopping.
</details>

* <details>
  <summary>
    <code>static void SetRecyclableAll(bool isRecyclable)</code>
  </summary>

  > Sets whether all updating `Tween`s are recyclable.
</details>

* <details>
  <summary>
    <code>static void RecycleAll()</code>
  </summary>

  > Stops all updating `Tween`s and recycles all unrecycled `Tween`s.
</details>

* <details>
  <summary>
    <code>static void Update()</code>
  </summary>

  > Updates all updating `Tween`s. Must be called every frame.
</details>

* <details>
  <summary>
    <code>static void DisposeAllNativeData()</code>
  </summary>

  > Disposes of all native data. Must be called when the application exits.
  > If native data is not disposed, it will cause editor errors upon application exit.
</details>