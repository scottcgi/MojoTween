## Overview

`TweenManager` controls all running Tweens and updates all Steps every frame. `Tween` controls both queued and concurrent TweenActions. `TweenAction` controls a list of TweenActionValues that will ease concurrently. `TweenActionValue` controls the target values. 

`Tween` is mainly used to build an action timeline, and then fill the timeline with actions. `TweenAction` can be created manually, but in most cases, the engine has extended a large number of existing `Unity Objects`.

Such as `Transform` can directly call the method with the `Action` prefix to create an `TweenAction`, just like `ActionMoveX(float x, float duration)`.

Both `Tween` and `TweenAction` support chained calls to set properties, including `ease`, `isRelative`, `callback` and more.

The detailed relationships of engine objects  can be found in the [Code Architecture](./CodeArchitecture.png) diagram.

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
  * [Control](#control)
  * [Test State](#test-state)
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
  * [Transform Shake Position](#transform-shake-position)
  * [Transform Shake Scale](#transform-shake-scale)
  * [Transform Shake Rotation](#transform-shake-rotation)
  * [Transform Bezier Quadratic Move](#transform-bezier-quadratic-move)
  * [Transform Bezier Quadratic Local Move](#transform-bezier-quadratic-local-move)
  * [Transform Bezier Cubic Move](#transform-bezier-cubic-move)
  * [Transform Bezier Cubic Local Move](#transform-bezier-cubic-local-move)
* [TweenManager](#tweenmanager)
* 


## Tween

#### Create Tween  
  
* <details>
  <summary>
    <code>static Tween Create(bool isRecyclable = true)</code>
  </summary>
  
  >Creates a Tween.
  >
  >If [isRecyclable] is true then the Tween will be auto recycled when it is completed — so don't hold a Tween and always create a new one.    
  >If [isRecyclable] is false then the Tween needs to be recycled manually by SetRecyclable — so the Tween can be Restart or Rewind.
  
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

#### Tween On Callback

* <details>
  <summary>
    <code>Tween SetOnStart(Action OnStart)</code>
  </summary>
  
  >Sets the [OnStart] callback which is called when the Tween starts (Play or Rewind).
 
  ```C#
  Tween.Create().SetOnStart(MyStartAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnComplete(Action OnComplete)</code>
  </summary>
  
  >Sets the [OnComplete] callback which is called when the Tween completes (Play or Rewind).
 
  ```C#
  Tween.Create().SetOnComplete(MyCompleteAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnStop(Action OnStop)</code>
  </summary>
  
  >Sets the [OnStop] callback which is called when the Tween stops (Play or Rewind).
 
  ```C#
  Tween.Create().SetOnStop(MyStopAction);
  ```
</details>

* <details>
  <summary>
    <code>Tween SetOnRecycle(Action OnRecycle)</code>
  </summary>
  
  >Sets the [OnRecycle] callback which is called when the Tween recycles.   
  >Tip: can use it to clear data bound to Tweens.
 
  ```C#
  Tween.Create().SetOnRecycle(MyRecycleAction);
  ```
</details>


#### Set Default Properties

* <details>
  <summary>
    <code>Tween SetDefaultEase(TweenEase ease)</code>
  </summary>
  
  >Sets the [ease] of Add or Append TweenAction, default Smooth.  
  >Only sets the TweenAction whose [ease] is Smooth.
 
  ```C#
  Tween.Create().SetDefaultEase(TweenEase.ExponentialOut);
  ``` 
</details>

* <details>
  <summary>
    <code>Tween SetDefaultRelative(bool isRelative)</code>
  </summary>
  
  >Sets the [isRelative] of Add or Append TweenActions, default false.  
  >Only sets the TweenAction whose [isRelative] is false.
 
  ```C#
  Tween.Create().SetDefaultRelative(true);
  ``` 
</details>

#### Control

* <details>
  <summary>
    <code>Tween SetRecyclable(bool isRecyclable)</code>
  </summary>
  
  >Sets the Tween to recyclable.  
  >If true and the Tween State is Setup or Completed or Stopped then recycle it immediately,
   else wait until it is completed and recycle it.
</details>

* <details>
  <summary>
    <code>Tween Play()</code>
  </summary>
  
  >Plays the Tween.
</details>

* <details>
  <summary>
    <code>Tween Rewind()</code>
  </summary>
  
  >Rewinds the Tween.  
  >The Tween cannot be recyclable!
</details>

* <details>
  <summary>
    <code>Tween Restart()</code>
  </summary>
  
  >Restarts the Tween (Play or Rewind).  
  >The Tween cannot be recyclable!
</details>

* <details>
  <summary>
    <code>Tween GotoStart()</code>
  </summary>
  
  >Goto the start of Tween (Play or Rewind).  
  >The Tween cannot be recyclable!
</details>

* <details>
  <summary>
    <code>Tween GotoEnd()</code>
  </summary>
  
  >Goto the end of Tween (Play or Rewind). The Tween cannot be recyclable!
</details>

* <details>
  <summary>
    <code>Tween Reverse()</code>
  </summary>
  
  >Reverses the Tween (Play or Rewind).  
  >If Tween is Completed then reverse the previous Play or Rewind,
   else reverse the Playing or Rewinding.
</details>

* <details>
  <summary>
    <code>Tween Stop()</code>
  </summary>
  
  >Stops the Tween Playing or Rewinding.  
  >If the Tween is recyclable then it will be recycled.
</details>

* <details>
  <summary>
    <code>void Pause(bool isPause)</code>
  </summary>
  
  >Pauses or resumes the Tween Playing or Rewinding.
</details>

#### Test State

* <details>
  <summary>
    <code>bool IsSetup()</code>
  </summary>
  
  >Whether the Tween state is Setup?
</details>

* <details>
  <summary>
    <code>bool IsPlaying()</code>
  </summary>
  
  >Whether the Tween state is Playing?
</details>

* <details>
  <summary>
    <code>bool IsRewinding()</code>
  </summary>
  
  >Whether the Tween state is Rewinding?
</details>

* <details>
  <summary>
    <code>bool IsRunning()</code>
  </summary>
  
  >Whether the Tween state is Playing or Rewinding? 
</details>

* <details>
  <summary>
    <code>bool IsPaused()</code>
  </summary>
  
  >Whether the Tween state is Paused? 
</details>

* <details>
  <summary>
    <code>bool IsStopping()</code>
  </summary>
  
  >Whether the Tween state is Stopping? 
</details>

* <details>
  <summary>
    <code>bool IsStopped()</code>
  </summary>
  
  >Whether the Tween state is Stopped? 
</details>

* <details>
  <summary>
    <code>bool IsStoppedByPlay()</code>
  </summary>
  
  >Whether the Tween state is Stopped by play? 
</details>

* <details>
  <summary>
    <code>bool IsStoppedByRewind()</code>
  </summary>
  
  >Whether the Tween state is Stopped by rewind?
</details>

* <details>
  <summary>
    <code>bool IsCompleted()</code>
  </summary>
  
  >Whether the Tween state is Completed?
</details>

* <details>
  <summary>
    <code>bool IsCompletedByPlay()</code>
  </summary>
  
  >Whether the Tween state is Completed by play? 
</details>

* <details>
  <summary>
    <code>bool IsCompletedByRewind()</code>
  </summary>
  
  >Whether the Tween state is Completed by rewind? 
</details>

* <details>
  <summary>
    <code>bool IsRecycled()</code>
  </summary>
  
  >Whether the Tween is Recycled? 
</details>

* <details>
  <summary>
    <code>bool IsOPRestart()</code>
  </summary>
  
  >Whether the Tween operation is Restart?  
  >Uses in TweenAction callback. 
</details>

* <details>
  <summary>
    <code>bool IsOPGotoStart()</code>
  </summary>
  
  >Whether the Tween operation is GotoStart?  
  >Uses in TweenAction callback. 
</details>

* <details>
  <summary>
    <code>bool IsOPGotoEnd()</code>
  </summary>
  
  >Whether the Tween operation is GotoEnd?  
  >Uses in TweenAction callback. 
</details>

## TweenExtensions

* <details>
  <summary>
    <code>void TogglePause()</code>
  </summary>
  
  >Toggles the Tween state between Playing or Rewinding and Pausing.
</details>

* <details>
  <summary>
    <code>Tween SetOnStartByPlay(Action OnStartByPlay)</code>
  </summary>
  
  >Sets the callback when the Tween starts by play.
</details>

* <details>
  <summary>
    <code>Tween SetOnStartByRewind(Action OnStartByRewind)</code>
  </summary>
  
  >Sets the callback when the Tween starts by rewind.
</details>

* <details>
  <summary>
    <code>Tween SetOnStopByPlay(Action OnStopByPlay)</code>
  </summary>
  
  >Sets the callback when the Tween stops by play.
</details>

* <details>
  <summary>
    <code>Tween SetOnStopByRewind(Action OnStopByRewind)</code>
  </summary>
  
  >Sets the callback when the Tween stops by rewind.
</details>

* <details>
  <summary>
    <code>Tween SetOnCompleteByPlay(Action OnCompleteByPlay)</code>
  </summary>
  
  >Sets the callback when the Tween completes by play.
</details>

* <details>
  <summary>
    <code>Tween SetOnCompleteByRewind(Action OnCompleteByRewind)</code>
  </summary>
  
  >Sets the callback when the Tween completes by rewind.
</details>

* <details>
  <summary>
    <code>Tween SetOnCompleteByRewind(Action OnCompleteByRewind)</code>
  </summary>
  
  >Sets the callback when the Tween completes by rewind.
</details>

* <details>
  <summary>
    <code>Tween SetLoops(int loops, bool isRewindToStart = false, Action OnCompleteByLoops = null)</code>
  </summary>
  
  >Sets the number of Tween play repeats.  
  >Repeated calls will add [loops] and [OnCompleteByLoops].  
  >
  >[loops]            : -1 means infinite loops.  
  >[isRewindToStart]  : Whether the Tween to rewinds when a loop is completed? — false to restart.  
  >[OnCompleteByLoops]: Callback when all loops are completed.  
 
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
    <code>static TweenAction CreateFloat(Func<float> OnGetTargetFloat, Action<float> OnSetTargetFloat, float finalValue, float duration)</code>
  </summary>
  
  >Creates a TweenAction ease to float.
     
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
  
  >Creates a TweenAction ease to vector2.
     
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
  
  >Creates a TweenAction ease to vector3. 
     
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
  
  >Creates a TweenAction ease to vector4.
     
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
  
  >Sets the [OnStart] callback which is called when the TweenAction starts (Play or Rewind).
</details>
   
* <details>
  <summary>
    <code>TweenAction SetOnComplete(Action OnComplete)</code>
  </summary>
  
  >Sets the [OnComplete] callback which is called when the TweenAction completes (Play or Rewind).
</details>     
   
#### Set Properties
   
* <details>
  <summary>
    <code>TweenAction SetRelative(bool isRelative)</code>
  </summary>
  
  >Sets the [isRelative] of all TweenActionValues, default false.
</details>      
   
* <details>
  <summary>
    <code>TweenAction SetRelativeAt(int index, bool isRelative)</code>
  </summary>
  
  >Sets the [isRelative] of TweenActionValue at index, default false.
</details>      
   
* <details>
  <summary>
    <code>TweenAction SetEase(TweenEase ease)</code>
  </summary>
  
  >Sets the [ease] of all TweenActionValues, default Smooth.
</details>
   
* <details>
  <summary>
    <code>TweenAction SetEaseAt(int index, TweenEase ease)</code>
  </summary>
  
  >Sets the [ease] of TweenActionValue at index, default Smooth.
</details>
   
* <details>
  <summary>
    <code>TweenAction SetExtraParams(params float[] extraParams)</code>
  </summary>
  
  >Sets the [extraParams] to TweenEase.
 
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
  
  >Plays the TweenAction.
  
  ```C#
  audioSource.ActionVolumeTo(toValue, duration).Play();
  ```
</details>
   
* <details>
  <summary>
    <code>Tween PlayDelay(float delay)</code>
  </summary>
  
  >Plays the delay TweenAction.
</details>     
   
## TweenActionAPIExtensions
   
#### Transform Move
   
* <details>
  <summary>
    <code>TweenAction ActionMoveX/Y/Z(float x/y/z, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position x/y/z to [x/y/z].
</details>   
   
* <details>
  <summary>
    <code>TweenAction ActionMoveXY(in Vector2 v2/float x, float y/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xy to [v2]/[xy]/[target].
</details>

* <details>
  <summary>
    <code>TweenAction ActionMoveXZ(in Vector2 v2/float x, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xz to [v2]/[xz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionMoveYZ(in Vector2 v2/float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position yz to [v2]/[yz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionMove(in Vector3 v3/float x, float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position to [v3]/[xyz]/[target].
</details>
   
#### Transform Local Move
   
* <details>
  <summary>
    <code>TweenAction ActionLocalMoveX/Y/Z(float x/float y/float z, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition x/y/z to [x]/[y]/[z].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalMoveXY(in Vector2 v2/float x, float y/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xy to [v2]/[xy]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalMoveXZ(in Vector2 v2/float x, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xz to [v2]/[xz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalMoveYZ(in Vector2 v2/float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition yz to [v2]/[yz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalMove(in Vector3 v3/float x, float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition to [v3]/[xyz]/[target].
</details>    
   
#### Transform Scale 

* <details>
  <summary>
    <code>TweenAction ActionScaleX/Y/Z(float x/float y/float z, float duration)</code>
  </summary>
  
  >Creates a TweenAction that scales the transform localScale x/y/z to [x]/[y]/[z].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionScaleXY(in Vector2 v2/float x, float y/float value/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that scales the transform localScale xy to [v2]/[xy]/[value]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionScaleXZ(in Vector2 v2/float x, float z/float value/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that scales the transform localScale xz to [v2]/[xz]/[value]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionScaleYZ(in Vector2 v2/float y, float z/float value/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that scales the transform localScale yz to [v2]/[yz]/[value]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionScale(in Vector3 v3/float x, float y, float z/float value/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that scales the transform localScale to [v3]/[xyz]/[value]/[target].
</details>
   
#### Transform Rotate   
   
* <details>
  <summary>
    <code>TweenAction ActionRotateX/Y/Z(float x/float y/float z, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform eulerAngles x/y/z to [x]/[y]/[z].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionRotateXY(in Vector2 v2/float x, float y/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform eulerAngles xy to [v2]/[xy]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionRotateXZ(in Vector2 v2/float x, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform eulerAngles xz to [v2]/[xz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionRotateYZ(in Vector2 v2/float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform eulerAngles yz to [v2]/[xz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionRotate(in Vector3 v3/float x, float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform eulerAngles to [v3]/[xyz]/[target].
</details>
   
#### Transform Local Rotate
   
* <details>
  <summary>
    <code>TweenAction ActionLocalRotateX/Y/Z(float x/float y/float z, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform localEulerAngles x/y/z to [x]/[y]/[z].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalRotateXY(in Vector2 v2/float x, float y/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform localEulerAngles xy to [v2]/[xy]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalRotateXZ(in Vector2 v2/float x, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform localEulerAngles xz to [v2]/[xz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalRotateYZ(in Vector2 v2/float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform localEulerAngles yz to [v2]/[yz]/[target].
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionLocalRotate(in Vector3 v3/float x, float y, float z/Transform target, float duration)</code>
  </summary>
  
  >Creates a TweenAction that rotates the transform localEulerAngles to [v3]/[xyz]/[target].
</details>
   
#### Transform Shake Position
   
* <details>
  <summary>
    <code>TweenAction ActionShakePositionX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform position x/y/z by [amplitude] and [speed].  
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionShakePositionXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform position xy/xz/yz by [amplitude] and [speed].  
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionShakePosition(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform position by [amplitude] and [speed].  
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
#### Transform Shake Scale
   
* <details>
  <summary>
    <code>TweenAction ActionShakeScaleX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform scale x/y/z by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionShakeScaleXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform scale xy/xz/yz by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionShakeScale(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform scale by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
#### Transform Shake Rotation
   
* <details>
  <summary>
    <code>TweenAction ActionShakeRotationX/Y/Z(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform rotation x/y/z by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details> 
   
* <details>
  <summary>
    <code>TweenAction ActionShakeRotationXY/XZ/YZ(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform rotation xy/xz/yz by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionShakeRotation(float amplitude, float speed, float duration)</code>
  </summary>
  
  >Creates a TweenAction that shakes the transform rotation by [amplitude] and [speed].   
  >Note: don't change the isRelative and TweenEase of this TweenAction.
</details>
   
#### Transform Bezier Quadratic Move
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveXY(in Vector2 v2/float x, float y/Transform target, in Vector2 controlPos/float controlPosX, float controlPosY/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xy to [v2]/[xy]/[target] by bezier2 with [controlPos].    
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveXZ(in Vector2 v2/float x, float z/Transform target, in Vector2 controlPos/float controlPosX, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xz to [v2]/[xz]/[target] by bezier2 with [controlPos].    
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2MoveYZ(in Vector2 v2/float y, float z/Transform target, in Vector2 controlPos/float controlPosY, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position yz to [v2]/[yz]/[target] by bezier2 with [controlPos].    
  >Note: don't change the TweenEase of this TweenAction.
</details>     
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2Move(in Vector3 v3/float x, float y, float z/Transform target, in Vector3 controlPos/float controlPosX, float controlPosY, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position to [v3]/[xyz]/[target] by bezier2 with [controlPos].      
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
#### Transform Bezier Quadratic Local Move
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveXY(in Vector2 v2/float x, float y/Transform target, in Vector2 controlPos/float controlPosX, float controlPosY/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xy to [v2]/[xy]/[target] by bezier2 with [controlPos].  
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveXZ(in Vector2 v2/float x, float z/Transform target, in Vector2 controlPos/float controlPosX, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xz to [v2]/[xz]/[target] by bezier2 with [controlPos].    
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMoveYZ(in Vector2 v2/float y, float z/Transform target, in Vector2 controlPos/float controlPosY, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition yz to [v2]/[yz]/[target] by bezier2 with [controlPos].    
  >Note: don't change the TweenEase of this TweenAction.
</details>     
   
* <details>
  <summary>
    <code>TweenAction ActionBezier2LocalMove(in Vector3 v3/float x, float y, float z/Transform target, in Vector3 controlPos/float controlPosX, float controlPosY, float controlPosZ/Transform controlPos, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition to [v3]/[xyz]/[target] by bezier2 with [controlPos].      
  >Note: don't change the TweenEase of this TweenAction.
</details>   
   
#### Transform Bezier Cubic Move
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveXY(in Vector2 v2/float x, float y/Transform target, in Vector2 controlPos1/float controlPos1X, float controlPos1Y/Transform controlPos1, in Vector2 controlPos2/float controlPos2X, float controlPos2Y/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xy to [v2]/[xy]/[target] by bezier3 with [controlPos1] and [controlPos2].   
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveXZ(in Vector2 v2/float x, float z/Transform target, in Vector2 controlPos1/float controlPos1X, float controlPos1Z/Transform controlPos1, in Vector2 controlPos2/float controlPos2X, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position xz to [v2]/[xz]/[target] by bezier3 with [controlPos1] and [controlPos2].    
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3MoveYZ(in Vector2 v2/float y, float z/Transform target, in Vector2 controlPos1/float controlPos1Y, float controlPos1Z/Transform controlPos1, in Vector2 controlPos2/float controlPos2Y, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position yz to [v2]/[yz]/[target] bezier3 with [controlPos1] and [controlPos2].     
  >Note: don't change the TweenEase of this TweenAction.
</details>     
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3Move(in Vector3 v3/float x, float y, float z/Transform target, in Vector3 controlPos1/float controlPos1X, float controlPos1Y, float controlPos1Z/Transform controlPos1, in Vector3 controlPos2/float controlPos2X, float controlPos2Y, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform position to [v3]/[xyz]/[target] by bezier3 with [controlPos1] and [controlPos2].    
  >Note: don't change the TweenEase of this TweenAction.
</details>   
   
#### Transform Bezier Cubic Local Move
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveXY(in Vector2 v2/float x, float y/Transform target, in Vector2 controlPos1/float controlPos1X, float controlPos1Y/Transform controlPos1, in Vector2 controlPos2/float controlPos2X, float controlPos2Y/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xy to [v2]/[xy]/[target] by bezier3 with [controlPos1] and [controlPos2].   
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveXZ(in Vector2 v2/float x, float z/Transform target, in Vector2 controlPos1/float controlPos1X, float controlPos1Z/Transform controlPos1, in Vector2 controlPos2/float controlPos2X, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition xz to [v2]/[xz]/[target] by bezier3 with [controlPos1] and [controlPos2].    
  >Note: don't change the TweenEase of this TweenAction.
</details>
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMoveYZ(in Vector2 v2/float y, float z/Transform target, in Vector2 controlPos1/float controlPos1Y, float controlPos1Z/Transform controlPos1, in Vector2 controlPos2/float controlPos2Y, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition yz to [v2]/[yz]/[target] bezier3 with [controlPos1] and [controlPos2].     
  >Note: don't change the TweenEase of this TweenAction.
</details>     
   
* <details>
  <summary>
    <code>TweenAction ActionBezier3LocalMove(in Vector3 v3/float x, float y, float z/Transform target, in Vector3 controlPos1/float controlPos1X, float controlPos1Y, float controlPos1Z/Transform controlPos1, in Vector3 controlPos2/float controlPos2X, float controlPos2Y, float controlPos2Z/Transform controlPos2, float duration)</code>
  </summary>
  
  >Creates a TweenAction that moves the transform localPosition to [v3]/[xyz]/[target] by bezier3 with [controlPos1] and [controlPos2].    
  >Note: don't change the TweenEase of this TweenAction.
</details>      
   
   

## TweenManager

* <details>
  <summary>
    <code>static bool IsAnyUpdating()</code>
  </summary>
  
  >Is there any Tween updating?
</details>

* <details>
  <summary>
    <code>static void StopAll()</code>
  </summary>

  >Stops all updating Tweens Playing or Rewinding.  
  >If the Tween is recyclable then it will be recycled on completion.
</details>

* <details>
  <summary>
    <code>static void RestartAll()</code>
  </summary>

  >Restarts all updating Tweens Playing or Rewinding.
</details>

* <details>
  <summary>
    <code>static void ReverseAll()</code>
  </summary>

  >Reverses all updating Tweens Playing or Rewinding.
</details>

* <details>
  <summary>
    <code>static void RewindAll()</code>
  </summary>

  >Rewinds all updating Tweens Playing or Rewinding.
</details>

* <details>
  <summary>
    <code>static PauseAll(bool isPause)</code>
  </summary>

  >Pauses or resumes all updating Tweens Playing or Rewinding.
</details>

* <details>
  <summary>
    <code>static void TogglePauseAll()</code>
  </summary>

  >Toggles all updating Tweens state between Playing or Rewinding and Paused.
</details>

* <details>
  <summary>
    <code>static void SetRecyclableAll(bool isRecyclable)</code>
  </summary>

  >Sets all updating Tweens to recyclable.
</details>

* <details>
  <summary>
    <code>static void RecycleAll()</code>
  </summary>

  >Stops all updating Tweens and Recycles all unrecycled Tweens.
</details>

* <details>
  <summary>
    <code>static void Update()</code>
  </summary>

  >Updates all Tweens, called every frame.
</details>

* <details>
  <summary>
    <code>static void DisposeAllNativeData()</code>
  </summary>

  >Disposes all native data with Allocator.Persistent, called when ApplicationQuit.  
  >If not dispose the native data, calling the Finalize of DisposeSentinel by GC, 
   will cause an editor error when app quit.
</details>

