## Overview

`TweenManager` controls all running Tweens and updates all Steps every frame. `Tween` controls both queued and concurrent TweenActions. `TweenAction` controls a list of TweenActionValues that will ease concurrently. `TweenActionValue` controls the target values. 

`Tween` is mainly used to build an action timeline, and then fill the timeline with actions. `TweenAction` can be created manually, but in most cases, the engine has extended a large number of existing `Unity Objects`.

Such as `Transform` can directly call the method with the `Action` prefix to create an `TweenAction`, just like `ActionMoveX(float x, float duration)`.

Both `Tween` and `TweenAction` support chained calls to set properties, including `ease`, `isRelative`, `callback` and more.

The detailed relationships of engine objects  can be found in the [Code Architecture](./CodeArchitecture.png) diagram.

* [Tween](#tween)
  * [Static](#static) 
  * [Append](#append)
  * [Add](#add)
  * [Add Delay](#add-delay)
  * [Add After](#add-after)
  * [Add Callback](#add-callback)
  * [Add Delay Callback](#add-delay-callback)
  * [On Callback](#on-callback)
* [TweenManager](#tweenmanager)
* 


## Tween

#### Static 
  
* <details>
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

* <details>
  <summary>
    <code>static Tween PlayDelayCallback(float delay, Action Callback)</code>
  </summary>
  
  >Plays the callback with delay time.
  
  ```C#
  Tween.PlayDelayCallback(1.0f, MyDelayCallbackAction);
  ```
</details>

#### Append 

* <details>
  <summary>
    <code>Tween Append(TweenAction action)</code>
  </summary>
  
  >Appends the TweenAction to the queue.
  
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
  
  >Appends the interval time to the queue.
  
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
  
  >Appends the callback to the queue.
  
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
  
  >Appends the interval time with callback to the queue.
  
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
  
  >Adds the TweenAction to the concurrent array.
  
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
  
  >Adds the delay TweenAction to the concurrent array.
  
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
  
  >Adds the delay TweenAction after the last Appended to the concurrent array. 
 
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
  
  >Adds the delay TweenAction after the last Added to the concurrent array.
 
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
  
  >Adds the TweenAction after the last Appended to the concurrent array.
  
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
  
  >Adds the TweenAction after the last Added to the concurrent array.
  
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
  
  >Adds the callback to the concurrent array.
 
  ```C#
  Tween.Create()
       .AddCallback(MyConcurrentCallbackAction1)
       .AddCallback(MyConcurrentCallbackAction2)
       .Play(); 
  ```
</details> 
        
</details>

* <details>
  <summary>
    <code>Tween AddCallbackAfterAppend(Action Callback)</code>
  </summary>
  
  >Adds the callback after the last Appended to the concurrent array.
 
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
  
  >Adds the callback after the last Added to the concurrent array.
 
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
  
  >Adds the delay callback to the concurrent array.
  
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
  
  >Adds the delay callback after the last Appended to the concurrent array.
 
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
  
  >Adds the delay callback after the last Added to the concurrent array.
  
  ```C#
  Tween.Create()
       .Add(MyConcurrentTweenAction1)
       .AddDelayCallbackAfterAdd(1.0f, MyConcurrentCallbackAction1)
       .Play();
  ```
</details>

#### On Callback

<details>
  <summary>
    <code>Tween SetOnStart(Action OnStart)</code>
  </summary>
  
  >Sets the [OnStart] callback which is called when the Tween starts (Play or Rewind).
 
  ```C#
  Tween.Create().SetOnStart(MyStartAction);
  ```
</details>

<details>
  <summary>
    <code>Tween SetOnComplete(Action OnComplete)</code>
  </summary>
  
  >Sets the [OnComplete] callback which is called when the Tween completes (Play or Rewind).
 
 ```C#
 Tween.Create().SetOnComplete(MyCompleteAction);
 ```
</details>

<details>
  <summary>
    <code>Tween SetOnStop(Action OnStop)</code>
  </summary>
  
  >Sets the [OnStop] callback which is called when the Tween stops (Play or Rewind).
 
  ```C#
  Tween.Create().SetOnStop(MyStopAction);
  ```
</details>

<details>
  <summary>
    <code>Tween SetOnRecycle(Action OnRecycle)</code>
  </summary>
  
  >Sets the [OnRecycle] callback which is called when the Tween recycles. 
  >
  >Tip: can use it to clear data bound to Tweens.
 
  ```C#
  Tween.Create().SetOnRecycle(MyRecycleAction);
  ```
</details>


## TweenManager

<details>
  <summary>
    <code>static bool IsAnyUpdating()</code>
  </summary>
  
  >Is there any Tween updating?
</details>

<details>
  <summary>
    <code>static void StopAll()</code>
  </summary>

  >Stops all updating Tweens Playing or Rewinding.
  >
  >If the Tween is recyclable then it will be recycled on completion.
</details>

<details>
  <summary>
    <code>static void RestartAll()</code>
  </summary>

  >Restarts all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>static void ReverseAll()</code>
  </summary>

  >Reverses all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>static void RewindAll()</code>
  </summary>

  >Rewinds all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>static void PauseAll()</code>
  </summary>

  >Pauses or resumes all updating Tweens Playing or Rewinding.
</details>

<details>
  <summary>
    <code>static void TogglePauseAll()</code>
  </summary>

  >Toggles all updating Tweens state between Playing or Rewinding and Paused.
</details>

<details>
  <summary>
    <code>static void SetRecyclableAll(bool isRecyclable)</code>
  </summary>

  >Sets all updating Tweens to recyclable.
</details>

<details>
  <summary>
    <code>static void RecycleAll()</code>
  </summary>

  >Stops all updating Tweens and Recycles all unrecycled Tweens.
</details>

<details>
  <summary>
    <code>static void Update()</code>
  </summary>

  >Updates all Tweens, called every frame.
</details>

<details>
  <summary>
    <code>static void DisposeAllNativeData()</code>
  </summary>

  >Disposes all native data with [Allocator.Persistent], called when [ApplicationQuit]. 
  >
  >If not dispose the native data, calling the [Finalize] of [DisposeSentinel] by GC, will cause an editor error when app quit.
</details>

