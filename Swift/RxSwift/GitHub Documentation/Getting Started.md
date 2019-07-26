# 시작하기

이 프로젝트는 [reactivex.io](http://reactivex.io/)와 일관성을 유지하기 위해 노력합니다. 일반적인 크로스 플랫폼 문서와 튜토리얼 또한 `RxSwift` 의 경우에도 유효할 것입니다.

## 시퀀스라고 불리는 옵저버블

#### Observables aka Sequences

### 기초

옵저버 패턴(`Observable<Element>`)과 일반적인 시퀀스(`Sequence`)의 동등함은 Rx를 이해하기 위한 가장 중요한 것입니다.

**모든 `Observable` 시퀀스는 단지 시퀀스일 뿐입니다. Swift의 `Sequence` 와 비교하여 `Observable` 의 주요한 이점은 그것이 요소를 비동기적으로도 받을 수 있다는 것입니다. 이것이 RxSwift의 핵심이며, 이 문서에 있는 것들은 그 아이디어를 확장하는 방식에 대한 것입니다.**

- `Observable`(`ObservableType`)은 `Sequence` 와 동등합니다.
- `ObservableType.subscribe` 메소드는 `Sequence.makeIterator` 메소드와 동등합니다.
- 옵저버(콜백)는, 반환된 반복자*iterator*에 `next()` 를 호출하는 대신, 시퀀스 요소를 받기 위해 `ObservableType.subscribe` 메소드에 넘겨질 필요가 있습니다.

시퀀스는 **시각화하기 쉬운** 간단하고 익숙한 개념입니다.

사람들은 거대한 시각령*visual cortex*을 가진 창조물입니다. 우리가 어떠한 개념을 쉽게 시각화할 때 이해하기 더 쉬워집니다.

우리는 모든 Rx 오퍼레이터 내부에 있는 이벤트 상태 머신을 시퀀스에서의 고수준 오퍼레이션으로 시뮬레이팅하여 많은 인지적 부하를 줄일 수 있습니다.

우리가 Rx를 사용하지 않고 비동기 시스템을 모델링한다면, 이는 아마 우리의 코드가 추상화되는 대신에 시뮬레이션할 필요가 있는 상태 머신과 일시적 상태로 가득차 있다는 것을 의미할 것입니다.

리스트와 시퀀스는 아마 수학자와 프로그래머가 배우는 첫 번째 개념 중 하나일 것입니다.

여기 숫자 시퀀스가 있습니다.

```text
--1--2--3--4--5--6| // 정상적으로 종료됨
```

문자로 이루어진 또다른 시퀀스가 있습니다.

```text
--a--b--a--a--a---d---X // 에러와 함께 종료됨
```

몇몇 시퀀스는 유한한 반면, 버튼을 탭하는 시퀀스와 같은 다른 것들은 무한합니다.

```text
---tap-tap-------tap--->
```

이런 것들을 마블 다이어그램이라고 부릅니다. [rxmarbles.com](http://rxmarbles.com/)에 더 많은 마블 다이어그램이 있습니다.

우리가 정규 표현식으로 시퀀스 문법을 명시한다면 다음과 같을 것입니다.

**next* (error | completed)?**

이는 다음의 것을 나타냅니다.

- **시퀀스는 0개 또는 그 이상의 요소를 가질 수 있습니다.**
- **`error` 나 `completed` 이벤트를 받는다면, 그 시퀀스는 어떠한 다른 요소를 생산할 수 없습니다.**

Rxd의 시퀀스는 (콜백이라 불리는) 밀어내기 인터페이스*push interface*에 의해 묘사됩니다.

```swift
enum Event<Element> {
  case next(Element)			// 시퀀스의 다음 요소
  case error(Swift.Error)	// 에러와 함께 실패한 시퀀스
  case completed					// 성공적으로 종료된 시퀀스
}

class Observable<Element> {
  func subscribe(_ observer: Observer<Element>) -> Disposable
}

protocol ObservableType {
  func on(_ event: Event<Element>)
}
```

**시퀀스가 `completed` 나 `error` 이벤트를 보낼 때 시퀀스 요소를 산출하는 모든 내부 리소스는 해제될 것입니다.**

**시퀀스 요소의 생산을 취소하고 리소스를 즉시 해제하려면 반환된 구독*subscription*에 `dispose` 를 호출하십시오.**

시퀀스가 유한한 시간 내에 종료된다면, `dispose` 를 호출하지 않거나 `disposed(by: disposeBag)` 을 사용하지 않는 것은 어떠한 영구적인 리소스 누수의 원인이 되지 않을지도 모릅니다. 하지만 그러한 리소스들은 시퀀스가 요소의 생산을 종료하든 에러를 반환하든, 완료될 때까지 사용될 것입니다.

일련의 버튼 탭과 같이, 시퀀스가 스스로 종료하지 않는다면, 리소스는 `dispose` 를 수동으로 호출하거나, `disposeBag`의 내부에 자동으로 있거나, `takeUntil` 오퍼레이터를 사용하거나, 다른 몇몇 방법을 사용하지 않는다면, 영구적으로 할당될 것입니다.

**dispose bag이나 `takeUntil` 오퍼레이터를 사용하는 것은 리소스가 정리되는 것을 보장하는 확실한 방법입니다. 시퀀스가 유한한 시간 내에 종료될지라도 그것들을 사용하는 것을 권장합니다.**

왜 `Swift.Error` 가 제네릭이 아닌지 궁금하다면, [여기](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/DesignRationale.md#why-error-type-isnt-generic)서 이에 대한 설명을 확인할 수 있습니다.

## Disposing

관찰되는 시퀀스가 종료할 수 있는 방법이 하나 더 있습니다. 우리가 시퀀스에 대한 작업을 완료하고 다가오는 요소를 계산하기 위해 할당된 모든 리소스를 해제하기 원한다면, 구독에 대해 `dispose` 를 호출할 수 있습니다.

`interval` 오퍼레이터에 대한 예제입니다.

```swift
let scheduler = SerialDispatchQueueScheduler(qos: .default)
let subscription = Observable<Int>.interval(.milliseconds(300), scheduler: scheduler)
  .subscribe { event in 
    print(event)
  }
Thread.sleep(forTimeInterval: 2.0)

subscription.dispose()
```

다음과 같이 출력될 것입니다.

```text
0
1
2
3
4
5
```

일반적으로 `dispose` 를 직접 호출하지 않기 원한다는 것에 주목하십시오. `dispose` 를 직접 호출하는 것은 보통 나쁜 코드의 악취를 풍깁니다. `DisposeBag`, `takeUntil` 오퍼레이터, 또는 다른 매커니즘을 사용하여 구독을 dispose하는 것이 더 낫습니다.

그래서 이 코드는 `dispose` 호출이 실행된 후에 어떠한 것을 출력할 수 있을까요? 답은, 그것에 달려 있습니다.

- `scheduler` 가 **시리얼 스케줄러** (예. `MainScheduler`)이고 `dispose` 가 **같은 시리얼 스케줄러**에서 호출되었다면, 답은 **아니오**입니다.
- 그렇지 않다면 답은 **예**입니다.

스케줄러에 대해서는 [여기](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/Schedulers.md)에서 더 알아볼 수 있습니다.

당신은 간단하게 병렬적으로 일어나는 두 개의 프로세스를 가지고 있습니다.

- 요소를 생산하는 프로세스
- 구독을 dispose하는 프로세스

"이후에 어떠한 것이 출력될 수 있는가?"의 질문은 그러한 프로세스들이 다른 스케줄러에 있다면 말이 되지도 않는 것입니다.

확실한 몇 가지 예가 있습니다. (`observeOn` 은 [여기](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/Schedulers.md)에서 설명합니다.)

다음의 경우,

```swift
let subscription = Observable<Int>.interval(.milliseconds(300), scheduler: scheduler)
  .observeOn(MainScheduler.instance)
  .subscribe { event in
    print(event)
  }

// ...

subscription.dispose()	// 메인 스레드에서 호출됨
```

**`dispose` 호출이 반환한 후 아무것도 출력되지 않습니다. 이는 보장됩니다.**

또한 다음의 경우,

```swift
let subscription = Observable<Int>.interval(.milliseconds(300), scheduler: scheduler)
  .observeOn(serialScheduler)
  .subscribe { event in
    print(event)
  }

// ...

subscription.dispose()	// 같은 `serialScheduler`에서 실행
```

**`dispose` 호출이 반환한 후 아무것도 출력되지 않습니다. 이는 보장됩니다.**

### Dispose Bags

dispose bag은 Rx에서 ARC와 같은 행위를 반환하기 위해 사용됩니다.

`DisposeBag` 이 해제될 때 추가된 각각의 disposables에 대하여 `dispose` 를 호출합니다.

이것은 `dispose` 메소드를 가지고 있지 않습니다. 그러므로 고의로 dispose를 명시적으로 호출할 수 없습니다. 즉각적인 정리가 필요하다면, 단지 새로운 bag를 만들면 됩니다.

```swift
self.disposeBag = DisposeBag()
```

이는 오래된 참조들을 정리하고 리소스의 정리를 불러 일으킬 것입니다.

명시적인 직접적 정리를 여전히 원한다면, `CompositeDisposable` 을 사용하십시오. **그것은 당신이 원하는 행위를 할 수 있게 하지만 `dispose` 메소드가 호출되면 즉시 새롭게 추가된 disposable을 정리할 것입니다.**

### Take until

dealloc에서 자동으로 구독을 정리하는 추가적인 방법은 `takeUntil` 오퍼레이터를 사용하는 것입니다.

```swift
sequence
  .takeUntil(self.rx.deallocated)
  .subscribe {
    print($0)
  }
```

## `Observable`이 암시적으로 보장하는 것

모든 시퀀스 생산자(`Observable`)가 추앙받아야 하는 두 가지의 추가적인 보장이 있습니다.

그것들이 어떤 스레드에서 요소를 생산하는지는 중요하지 않습니다. 하지만 하나의 요소를 생산하고 `observer.on(.next(nextElement))` 옵저버에게 그것을 보낸다면, `observer.on` 메소드가 실행을 완료할 때까지 다음 요소를 보낼 수 없습니다.

생산자는 또한 `.next` 이벤트가 아직 완료되지 않은 경우 `.completed` 나 `.error` 로 종료 이벤트를 보낼 수 없습니다.

말하자면, 다음의 예제가 있습니다.

```swift
someObservable
  .subscribe { event in 
    print("Event processing started")
    // 처리 중
    print("Event processing ended")
  }
```

이는 항상 다음의 것을 출력할 것입니다.

```text
Event processing started
Event processing ended
Event processing started
Event processing ended
Event processing started
Event processing ended
```

다음의 것을 절대 출력할 수 없습니다.

```text
Event processing started
Event processing started
Event processing ended
Event processing ended
```

## 자신만의 `Observable` (관찰 가능한 시퀀스) 만들기

observable에 대해 이해하기 위한 한 가지 중요한 것이 있습니다.

**observable이 만들어질 때, 그것은 어떠한 작업도 수행하지 않습니다. 단지 만들어졌기 때문입니다.**

`Observable` 이 많은 방식으로 요소를 생산할 수 있다는 것은 사실입니다. 그것들 중 몇몇은 사이드 이펙트를 불러 일으키고, 몇몇은 마우스 이벤트를 활용하는 것과 같이 기존에 실행 중인 프로세스를 활용합니다.

**그러나, `Observable` 을 반환하는 메소드를 호출한다면, 어떠한 시퀀스 생산도 수행되지 않고 사이드 이펙트도 발생하지 않습니다. `Observable` 은 단지 시퀀스가 생산되는 방법과 요소 생산에 어떠한 매개변수가 사용되는지를 정의할 뿐입니다. 시퀀스 생산은 `subscribe` 메소드가 호출될 때 시작합니다.**

예를 들어 비슷한 프로토타입을 가진 메소드가 있다고 합시다.

```swift
func searchWikipedia(searchTerm: String) -> Observable<Results> { }
```

```swift
let searchForMe = searchWikipedia("me")

// 어떠한 요청도 수행되지 않았다. 어떠한 작업도 완료되지 않았다. 어떠한 URL 요청도 발생하지 않았다.

let cancel = searchForMe
  // 시퀀스 생산은 지금 시작한다. URL 요청이 발생한다.
  .subscribe(onNext: { results in
    print(results)
  })
```

당신만의 `Observable` 시퀀스를 만드는 많은 방법이 있습니다. 가장 쉬운 방법은 `create` 함수를 사용하는 것일 것입니다.

RxSwift는 구독할 때 하나의 요소를 반환하는 시퀀스를 만드는 메소드를 제공합니다. 이 메소드는 `just` 라고 불립니다. 그것에 대한 구현을 작성해 봅시다.

*이것은 실제 구현입니다.*

```swift
func myJust<E>(_ element: E) -> Observable<E> {
  return Observable.create { observer in
    observer.on(.next(element))
    observer.on(.completed)
    return Disposables.create()
  }
}

myJust(0)
  .subscribe(onNext: { n in
    print(n)
  })
```

다음을 출력할 것입니다.

```text
0
```

나쁘지 않습니다. 그래서 `create` 함수는 무엇입니까?

이것은 Swift의 클로저를 사용하여 `subscribe` 메소드를 쉽게 구현할 수 있게 하는 편의 메소드일 뿐입니다. `subscribe` 메소드처럼 이것은 하나의 인자, `observer` 를 취하며, disposable을 반환합니다.

이러한 방식으로 구현된 시퀀스는 사실 동기적입니다. 이것은 `subscribe` 호출이 구독을 나타내는 disposable을 반환하기 전에 요소를 생산하고 종료될 것입니다. 이 때문에 그것이 반환하는 disposable이 무엇인지는 정말로 중요한 것이 아니며, 요소를 생산하는 프로세스는 중단될 수 없습니다.

동기적 시퀀스를 생산할 때, 일반적인 disposable이 반환하는 것은 `NopDisposable` 의 싱글턴 인스턴스입니다.

이제 배열로부터 요소를 반환하는 observable을 만들어 봅시다.

*이것은 실제 구현입니다.*

```swift
func myFrom<E>(_ sequence: [E]) -> Observable<E> {
  return Observable.create { observer in
    for element in sequence {
      observer.on(.next(element))
    }
    observer.on(.completed)
    return Disposables.create()
  }
}

let stringCounter = myFrom(["first", "second"])

print("Started ----")

// 첫 번째
stringCounter
  .subscribe(onNext: { n in
    print(n)
  })

print("----")

// 다시
stringCounter
  .subscribe(onNext: { n in 
    print(n)
  })

print("Ended ----")
```

다음을 출력할 것입니다.

```swift
Started ----
first
second
----
first
second
Ended ----
```

## 작업을 수행하는 `Observable` 만들기

좋습니다. 무언가 더 흥미로워지고 있습니다. 이전 예제에서 사용했던 `interval` 오퍼레이터를 만들어 봅시다.

*이는 디스패치 큐 스케줄러의 실제 구현과 동등합니다.*

```swift
func myInterval(_ interval: DispatchTimeInterval) -> Observable<Int> {
  return Observable.create { observer in
    print("Subscribed")
    let timer = DispatchSource.makeTimerSource(queue: DispatchQueue.global())
    timer.schedule(deadline: DispatchTime.now() + interval, repeating: interval)
                            
    let cancel = Disposables.create {
      print("Disposed")
      timer.cancel()
    }

    var next = 0
    timer.setEventHandler {
      if cancel.isDisposed {
        return
      }
      observer.on(.next(next))
      next += 1
    }
    timer.resume()
    
    return cancel
  }
}
```

```swift
let counter = myInterval(.milliseconds(100))

print("Started ----")

let subscription = counter
  .subscribe(onNext: { n in
    print(n)
  })


Thread.sleep(forTimeInterval: 0.5)

subscription.dispose()

print("Ended ----")
```

다음을 출력할 것입니다.

```text
Started ----
Subscribed
0
1
2
3
4
Disposed
Ended ----
```

다음과 같이 작성하면,

```swift
let counter = myInterval(.milliseconds(100))

print("Started ----")

let subscription1 = counter
  .subscribe(onNext: { n in
    print("First \(n)")
  })
let subscription2 = counter
  .subscribe(onNext: { n in
    print("Second \(n)")
  })

Thread.sleep(forTimeInterval: 0.5)

subscription1.dispose()

Thread.sleep(forTimeInterval: 0.5)

subscription2.dispose()

print("Ended ----")
```

다음을 출력할 것입니다.

```text
Started ----
Subscribed
Subscribed
First 0
Second 0
First 1
Second 1
First 2
Second 2
First 3
Second 3
First 4
Second 4
Disposed
Second 5
Second 6
Second 7
Second 8
Second 9
Disposed
Ended ----
```

**구독 중인 모든 구독자는 일반적으로 그것만의 개별 요소 시퀀스를 생산합니다. 오퍼레이터들은 기본적으로 상태가 없습니다. 상태가 있는 오퍼레이터보다는 상태가 없는 오퍼레이터가 훨씬 많습니다.**

## 구독과 `share` 오퍼레이터 공유

하지만 여러 개의 옵저버가 오직 하나의 구독으로부터 이벤트(요소)들을 공유하기 원한다면 어떻게 하시겠습니까?

정의될 필요가 있는 두 가지 것이 있습니다.

- 