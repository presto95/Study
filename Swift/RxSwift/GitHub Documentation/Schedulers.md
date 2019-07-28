# 스케줄러

#### Schedulers

스케줄러는 작업을 수행하기 위한 매커니즘을 추상화합니다.

작업을 수행하기 위한 다른 매커니즘은 현재 스레드, 디스패치 큐, 오퍼레이션 큐, 새로운 스레드, 스레드 풀, 런 루프를 포함합니다.

스케줄러와 함께 동작하는 두 개의 주요한 오퍼레이터, `observeOn` 과 `subscribeOn` 이 있습니다.

다른 스케줄러에서 작업을 수행하기 원한다면 `observeOn(scheduler)` 오퍼레이터를 사용하십시오.

일반적으로 `subscribeOn` 보다 `observeOn` 을 더 많이 사용하게 될 것입니다.

`observeOn` 이 명시되지 않은 경우, 작업은 요소가 생성된 스레드 / 스케줄러에서 수행될 것입니다.

`observeOn` 오퍼레이터를 사용하는 예제입니다.

```swift
sequence1
  .observeOn(backgroundScheduler)
  .map { n in
      print("This is performed on the background scheduler")
  }
  .observeOn(MainScheduler.instance)
  .map { n in
      print("This is performed on the main scheduler")
  }
```

특정 스케줄러에서 시퀀스 생성을 시작하고 (`subscribe` 메소드) dispose를 호출하고 싶다면 `subscribeOn(scheduler)` 를 사용하십시오.

`subscribeOn` 이 명시되지 않은 경우, `subscribe` 클로저 (`Observable.create` 에 전달된 클로저) 는 `subscribe(onNext:)` 나 `subscribe` 가 호출된 스레드 / 스케줄러와 같은 곳에서 호출될 것입니다.

`subscribeOn` 이 명시되지 않은 경우, `dispose` 메소드는 dispose가 발생한 스레드 / 스케줄러와 같은 곳에서 호출될 것입니다.

말하자면, 특정 스케줄러가 선택되지 않으면, 그러한 메소드는 현재 스레드 / 스케줄러에서 호출될 것입니다.

## 직렬 vs. 병렬 스케줄러

#### Serial vs. Concurrent Schedulers

스케줄러는 정말로 어떠한 것이든 될 수 있고, 시퀀스를 변형하는 모든 오퍼레이터는 부가적인 암시적 보장을 보존할 필요가 있기 때문에, 당신이 만드는 스케줄러가 어떤 종류인지는 중요합니다.

스케줄러가 병렬적인 경우, Rx의 `observeOn` 과 `subscribeOn` 오퍼레이터는 모든 것이 완벽하게 동작할 것이라는 것을 보장할 것입니다.

Rx가 직렬이라고 입증할 수 있는 스케줄러를 사용한다면, 부가적인 최적화를 수행할 수 있을 것입니다.

기껏해야 디스패치 큐 스케줄러에 대하여 최적화를 수행할 것입니다.

직렬 디스패치 큐 스케줄러의 경우 `observeOn` 은 간단한 `dispatch_async` 호출로 최적화됩니다.

## 커스텀 스케줄러

#### Custom Schedulers

현재 스케줄러에 더하여 당신만의 스케줄러를 작성할 수 있습니다.

작업을 즉시 수행하기 위해 필요로 하는 것을 묘사하기 원한다면, `ImmediateScheduler` 프로토콜을 구현하여 당신만의 스케줄러를 만들 수 있습니다.

```swift
public protocol ImmediateScheduler {
  func schedule<StateType>(state: StateType, action: (/*ImmediateScheduler,*/ StateType) -> RxResult<Disposable>) -> RxResult<Disposable>
}
```

시간에 기반한 작업을 지원하는 새로운 스케줄러를 만들고 싶다면, `Scheduler` 프로토콜을 구현할 필요가 있을 것입니다.

```swift
public protocol Scheduler: ImmediateScheduler {
  associatedtype TimeInterval
  associatedtype Time

  var now : Time { get }

  func scheduleRelative<StateType>(state: StateType, dueTime: TimeInterval, action: (StateType) -> RxResult<Disposable>) -> RxResult<Disposable>
}
```

스케줄러가 주기적인 스케줄링 능력만을 가지는 경우, `PeriodicScheduler` 프로토콜을 구현하여 Rx에게 알릴 수 있습니다.

```swift
public protocol PeriodicScheduler : Scheduler {
  func schedulePeriodic<StateType>(state: StateType, startAfter: TimeInterval, period: TimeInterval, action: (StateType) -> StateType) -> RxResult<Disposable>
}
```

스케줄러가 `PeriodicScheduling` 의 능력을 지원하지 않는 경우, Rx는 명백하게 주기적인 스케줄링을 에뮬레이팅할 것입니다.

## 내장 스케줄러

#### Builtin schedulers

Rx는 모든 타입의 스케줄러를 사용할 수 있으나, 스케줄러가 직렬이라는 증거가 있다면 어떠한 부가적인 최적화 과정 또한 수행할 수 있습니다.

현재 지원하는 스케줄러들입니다.

### CurrentThreadScheduler (직렬 스케줄러)

현재 스레드에서 작업 단위를 스케줄링합니다. 요소를 생산하는 오퍼레이터의 기본 스케줄러입니다.

이 스케줄러는 때때로 "트램펄린 스케줄러"라고도 불립니다.

`CurrentThreadScheduler.instance.schedule(state) { }` 가 어떤 스레드에서 최초로 호출될 때, 예정된 액션이 즉시 실행되고, 재귀적으로 스케줄링된 모든 액션이 일시적으로 담겨지는 숨겨진 큐가 생성될 것입니다.

호출 스택에서 몇몇 상위 프레임이 이미 `CurrentThreadScheduler.instance.schedule(state) { }` 에서 동작 중이라면, 예정된 액션은 큐에 들어가고 현재 진행 중인 액션과 이전에 큐에 담겨진 모든 액션이 실행을 마칠 때 실행될 것입니다.

### MainScheduler (직렬 스케줄러)

`MainThread` 에서 수행될 필요가 있는 작업을 추상화합니다. `schedule` 메소드가 메인 스레드에서 호출되는 경우, 해당 액션은 스케줄링되지 않고 즉시 수행될 것입니다.

이 스케줄러는 보통 UI 작업을 위해 사용됩니다.

### SerialDispatchQueueScheduler (직렬 스케줄러)

특정 `dispatch_queue_t` 에서 수행될 필요가 있는 작업을 추상화합니다. 병렬 디스패치 큐가 넘겨졌을지라도 직렬의 것으로 변환될 것임을 보장할 것입니다.

직렬 스케줄러는 `observeOn` 에 대하여 특정 최적화를 활성화합니다.

메인 스케줄러는 `SerialDispatchQueueScheduler` 의 인스턴스입니다.

### ConcurrentDispatchQueueScheduler (병렬 스케줄러)

특정 `dispatch_queue_t` 에서 수행될 필요가 있는 작업을 추상화합니다. 또한 직렬 디스패치 큐를 전달할 수 있으며 어떠한 문제도 발생시키지 않을 것입니다.

이 스케줄러는 어떠한 작업이 백그라운드에서 수행될 필요가 있을 때 적합합니다.

### OperationQueueScheduler (병렬 스케줄러)

특정 `NSOperationQueue` 에서 수행될 필요가 있는 작업을 추상화합니다.

이 스케줄러는 백그라운드에서 수행될 필요가 있는 어떠한 큰 조각의 작업이 있고, `maxConcurrentOperationCount` 를 사용하여 병렬 프로세싱을 조절하기 원할 때 적합합니다.