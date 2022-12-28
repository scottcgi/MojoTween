## Overview

`TweenManager` controls all running Tweens and updates all Steps every frame. `Tween` controls both queued and concurrent TweenActions. `TweenAction` controls a list of TweenActionValues that will ease concurrently. `TweenActionValue` controls the target values. 

`Tween` is mainly used to build an action timeline, and then fill the timeline with actions. `TweenAction` can be created manually, but in most cases, the engine has extended a large number of existing `Unity Objects`.

Such as `Transform` can directly call the method with the `Action` prefix to create an `TweenAction`, just like `ActionMoveX(float x, float duration)`.

Both `Tween` and `TweenAction` support chained calls to set properties, including `ease`, `isRelative`, `callback` and more.

The detailed relationships of engine objects  can be found in the [Code Architecture](./CodeArchitecture.png) diagram.

* [Tween](#tween)
* [TweenManager](#tweenmanager)
* 


## Tween

<details>
  <summary>
    <code>static Tween Create(bool isRecyclable = true)</code>
  </summary>
  
  >Creates a Tween.
  >
  >If [isRecyclable] is [true] then the Tween will be auto recycled when it is completed — so don't hold a Tween and always create a new one.
  >
  >If [isRecyclable] is [false] then the Tween needs to be recycled manually by [SetRecyclable] — so the Tween can be [Restart] or [Rewind].
  
  ```C#
  // auto recycle
  var tween = Tween.Create();
  // manual recycle
  var tween = Tween.Create(false);
  // recycle tween
  tween.SetRecyclable(true);
  ```
</details>

<details>
  <summary>
    <code>static Tween PlayDelayCallback(float delay, Action Callback)</code>
  </summary>
  
  >Plays the callback with delay time.
  
  ```C#
  Tween.PlayDelayCallback(1.0f, () => tween.Reverse());
  ```
</details>

<details>
  <summary>
    <code>Tween Append(TweenAction action)</code>
  </summary>
  
  >Appends the TweenAction to the queue.
  
  ```C#
  Tween.Create()             
       .Append(transform.ActionMoveY(1.0f, 0.6f))
       .Append(transform.ActionMoveY(0.5f, 1.2f))
       .Play();
  ```  
</details>

<details>
  <summary>
    <code>Tween AppendInterval(float interval)</code>
  </summary>
  
  >Appends the interval time to the queue.
  
  ```C#
  Tween.Create()
       .AppendInterval(0.8f)
       .Append(transform.ActionMoveY(1.0f, 0.6f))
       .AppendInterval(1.5f)
       .Append(transform.ActionMoveY(0.5f, 1.2f))
       .Play();
  ```    
</details>

<details>
  <summary>
    <code>Tween AppendCallback(Action Callback)</code>
  </summary>
  
  >Appends the callback to the queue.
  
  ```C#
  Tween.Create()
       .AppendInterval(0.8f)
       .AppendCallback(() => tween.Stop())
       .AppendInterval(1.5f)
       .AppendCallback(() => tween.SetRecyclable(true))
       .Play();
  ```     
</details>

<details>
  <summary>
    <code>Tween AppendIntervalCallback(float interval, Action Callback)</code>
  </summary>
  
  >Appends the interval time with callback to the queue.
  
  ```C#
  Tween.Create()
       .AppendIntervalCallback(0.7f, () => tween.Rewind().SetRecyclable(true))
       .Play();
  ```    
</details>

<details>
  <summary>
    <code>Tween Add(TweenAction action)</code>
  </summary>
  
  >Adds the TweenAction to the concurrent array.
  
  ```C#
  Tween.Create()
       .Add(transform.ActionRotateY(410f, 2.0f))
       .Add(transform.ActionScale  (0.5f, 2.0f))
       .Play();
  ```   
</details>

<details>
  <summary>
    <code>Tween AddWithDelay(float delay, TweenAction action)</code>
  </summary>
  
  >Adds the TweenAction with delay time to the concurrent array.
  
  ```C#
  Tween.Create()
       .AddWithDelay(0.0f, transform0.ActionMoveY(10.0f, 3.0f))
       .AddWithDelay(0.2f, transform1.ActionMoveY(20.0f, 3.0f))  
       .AddWithDelay(0.4f, transform2.ActionMoveY(30.0f, 3.0f))
       .Play();
  ```     
</details>

<details>
  <summary>
    <code>Tween AddDelayCallback(float delay, Action Callback)</code>
  </summary>
  
  >Adds the delay callback to the concurrent array.
  
  ```C#
  Tween.Create()
       .AddDelayCallback(1.0f, () => tween.Reverse())
       .AddDelayCallback(1.8f, () => tween.Reverse())
       .AddDelayCallback(2.3f, () => tween.Reverse())
       .AddDelayCallback(2.9f, () => tween.Reverse())
       .AddDelayCallback(3.4f, () => tween.Reverse().SetRecyclable(true))
       .Play();
  ```   
</details>

<details>
  <summary>
    <code>Tween AddAfterAppend(TweenAction action)</code>
  </summary>
  
  >Adds the TweenAction after the last Appended to the concurrent array.
  
  ```C#
  Tween.Create()
       .Append        (transform.ActionShakeRotationY(30.0f, 5.0f,  1.5f))
       .AddAfterAppend(transform.ActionMoveZ         (0.0f,  1.0f       ))
       .Append        (transform.ActionShakeRotationZ(60.0f, 12.0f, 1.5f))
       .Play();
  ```    
</details>

<details>
  <summary>
    <code>Tween AddAfterAppendWithDelay(float delay, TweenAction action)</code>
  </summary>
  
  >Adds the TweenAction after the last Appended with delay time to the concurrent array.
  
  ```C#
  Tween.Create()
       .Append(transform.ActionShakeRotationY(30.0f, 5.0f,  1.5f))
       .AddAfterAppendWithDelay(0.5f, transform.ActionMoveZ(0.0f,  1.0f))
       .Append(transform.ActionShakeRotationZ(60.0f, 12.0f, 1.5f))
       .Play();
  ```    
</details>

## TweenManager

<details>
  <summary>
    <code>bool IsAnyUpdating()</code>
  </summary>
  
  >Is there any Tween updating?
</details>

<details>
  <summary>
    <code>void StopAll()</code>
  </summary>

  >Stops all updating Tweens Playing or Rewinding.
  >
  >If the Tween is recyclable then it will be recycled on completion.
</details>

<details>
  <summary>
    <code>void RestartAll()</code>
  </summary>

  >Restarts all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>void ReverseAll()</code>
  </summary>

  >Reverses all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>void RewindAll()</code>
  </summary>

  >Rewinds all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>void PauseAll()</code>
  </summary>

  >Pauses or resumes all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>void TogglePauseAll()</code>
  </summary>

  >Toggles all updating Tweens state between Playing or Rewinding and Paused.
</details>


<details>
  <summary>
    <code>void SetRecyclableAll(bool isRecyclable)</code>
  </summary>

  >Sets all updating Tweens to recyclable.
</details>

<details>
  <summary>
    <code>void RecycleAll()</code>
  </summary>

  >Stops all updating Tweens and Recycles all unrecycled Tweens.
</details>

<details>
  <summary>
    <code>void Update()</code>
  </summary>

  >Updates all Tweens, called every frame.
</details>

<details>
  <summary>
    <code>DisposeAllNativeData()</code>
  </summary>

  >Disposes all native data with [Allocator.Persistent], called when [ApplicationQuit]. 
  >
  >If not dispose the native data, calling the [Finalize] of [DisposeSentinel] by GC, will cause an editor error when app quit.
</details>

