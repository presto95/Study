# ObserveOn

### 분류

Observable 유틸리티 연산자

### Observable 연산자 결정 트리

1. 연산자가 특정 스케줄러 상에서 동작해야 하며
2. 연산자가 옵저버에게 알림을 줄 때 동작할 스케줄러를 지정해야 한다면

---

옵저버가 Observable을 구독하는 Scheduler를 지정

---

### SubscribeOn과의 차이점

[공식 문서 한 장이면 끝장이다.](http://reactivex.io/documentation/ko/scheduler.html)

SubscribeOn은 스케줄러 상에 있는 옵저버뿐만 아니라, Observable 자체가 동작할 특정 스케줄러를 지정한다.

> 다른 스케줄러를 지정해서 Observable이 처리해야 할 연산자들을 실행시킨다.

ObserveOn은 옵저버가 동작할 스케줄러를 지정한다.

> Observable이 옵저버에게 알림을 보낼 때 사용할 스케줄러를 명시한다.

---

### Scheduler?

>  Observable 연산자 체인에 멀티스레딩을 적용하고 싶다면, 특정 스케줄러를 사용해서 연산자 또는 특정 Observable을 실행하면 된다.

RxSwift에는 다양한 스케줄러가 정의되어 있다.

- MainScheduler
  - `DispatchQueue.main` 에서 수행되는 작업의 추상
  - 보통 UI 작업을 위해 사용됨
  - SerialDispatchQueueScheduler의 한 종류
- HistoricalScheduler
  - 절대 시간을 위해 `Date`, 상대 시간을 위해 `NSTimeInterval` 을 사용하는 가상의 타임 스케줄러
  - `VirtualTimeScheduler<HistoricalSchedulerTimeConverter>`를 상속받음
- CurrentThreadScheduler
  - 기본 스케줄러
- OperationQueueScheduler
  - 특정 `NSOperationQueue` 에서 수행되는 작업의 추상
- ConcurrentMainScheduler
  - 메인 쓰레드에서 수행되는 작업의 추상
  - SubscribeOn 오퍼레이터에 최적화되어 있음
    - ObserveOn 오퍼레이터를 사용하여 메인 쓰레드에서 Observable을 옵저빙하기보다는, MainScheduler에서 Observable을 구독하는 것이 더 좋음
- SerialDispatchQueueScheduler
  - 직렬 디스패치 큐 스케줄러
  - `...sync`
- ConcurrentDispatchQueueScheduler
  - 병렬 디스패치 큐 스케줄러
  - `...async`

---

observeOn을 사용하고 다음 오퍼레이터부터 observeOn이 지정한 스케줄러에서 옵저빙이 동작한다.

---

DispatchQueue 활용을 더욱 쉽게 하는 느낌으로 사용하면 될 것 같다. UI 업데이트가 필요한 작업에는 메인 쓰레드로 스케줄러를 돌린다든지.

```swift
Observable.from([1, 2, 3])
  .observeOn(MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// completed
```

