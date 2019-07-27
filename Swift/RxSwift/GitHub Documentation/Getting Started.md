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

- 새로운 구독자가 그동안 받아온 과거의 요소들을 관찰하는 데 관심이 있기 전에 그것들을 다루는 방법 (가장 마지막의 것만 리플레이, 모두 리플레이, 마지막 n개의 것을 리플레이)
- 공유된 구독이 발생하는 때를 결정하는 것 (참조 횟수, 직접 또는 다른 알고리즘)

일반적인 선택은 `share(replay: 1)` 이라고 불리는 `replay(1).refCount()` 의 조합입니다.

```swift
let counter = myInterval(.milliseconds(100))
  .share(replay: 1)

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
First 5
Second 5
Second 6
Second 7
Second 8
Second 9
Disposed
Ended ----
```

어떻게 `Subscribed` 이벤트와 `Disposed` 이벤트가 오직 한 번 발생했는지에 주목하십시오.

URL observable에 대한 행위도 이와 동등합니다.

아래는 HTTP 요청이 Rx로 래핑된 방법을 보여줍니다. 이는 `interval` 오퍼레이터와 거의 같은 패턴을 갖습니다.

```swift
extension Reactive where Base: URLSession {
  public func response(request: URLRequest) -> Observable<(response: HTTPURLResponse, data: Data)> {
    return Observable.create { observer in
      let task = self.base.dataTask(with: request) { (data, response, error) in

        guard let response = response, let data = data else {
          observer.on(.error(error ?? RxCocoaURLError.unknown))
          return
        }

        guard let httpResponse = response as? HTTPURLResponse else {
            observer.on(.error(RxCocoaURLError.nonHTTPResponse(response: response)))
            return
        }

        observer.on(.next((httpResponse, data)))
        observer.on(.completed)
      }

      task.resume()

      return Disposables.create {
        task.cancel()
      }
    }
  }
}
```

## 오퍼레이터

#### Operators

RxSwift에 구현된 수많은 오퍼레이터들이 있습니다.

모든 오퍼레이터에 대한 마블 다이어그램을 [reactivex.io](http://reactivex.io/)에서 확인할 수 있습니다.

거의 모든 오퍼레이터들은 [플레이그라운드](http://reactivex.io/)에서 입증됩니다.

플레이그라운드를 사용하려면 `Rx.xcworkspace` 를 열고 `RxSwift-macOS` 스킴을 빌드한 후 `Rx.xcworkspace` 의 트리 뷰에서 플레이그라운드를 여십시오.

어떠한 오퍼레이터가 필요한 경우 그것을 찾는 방법을 모른다면 [오퍼레이터 결정 트리](http://reactivex.io/documentation/operators.html#tree)를 참고할 수 있습니다.

### 커스텀 오퍼레이터

커스텀 오퍼레이터를 만들 수 있는 두 가지 방법이 있습니다.

#### 쉬운 방법

내부에 있는 모든 코드는 매우 최적화된 버전의 오퍼레이터를 사용합니다. 그러므로 그것들은 최상의 튜토리얼 자료는 아닙니다. 이것이 표준 오퍼레이터를 사용하라고 매우 장려되는 이유입니다.

운좋게도 오퍼레이터를 만드는 더 쉬운 방법이 있습니다. 사실 새로운 오퍼레이터를 만드는 것은 observable을 만드는 것에 대한 것이고, 이전 챕터에서 이미 어떻게 하는지에 대해 설명했습니다.

최적화되지 않은 map 오퍼레이터가 구현되는 방법을 봅시다.

```swift
extension ObservableType {
  func myMap<R>(transform: @escaping (E) -> R) -> Observable<R> {
    return Observable.create { observer in
      let subscription = self.subscribe { e in
        switch e {
        case .next(let value):
          let result = transform(value)
          observer.on(.next(result))
        case .error(let error):
          observer.on(.error(error))
        case .completed:
          observer.on(.completed)
        }
      }

      return subscription
    }
  }
}
```

당신만의 map을 다음과 같이 사용할 수 있습니다.

```swift
let subscription = myInterval(.milliseconds(100))
  .myMap { e in 
    return "This is simply \(e)"
  }
  .subscribe(onNext: { n in
    print(n)
  })
```

다음을 출력할 것입니다.

```text
Subscribed
This is simply 0
This is simply 1
This is simply 2
This is simply 3
This is simply 4
This is simply 5
This is simply 6
This is simply 7
This is simply 8
...
```

### Life happens

커스텀 오퍼레이터를 사용해서 몇몇 케이스를 해결하는 것이 너무 어렵다면 어떻게 하시겠습니까? Rx 모나드를 탈출해서 명령형 세계에서 액션을 수행하고 `Subject` 를 사용하여 Rx로 되돌아와 결과를 가져올 수 있습니다.

아래는 실용적인 것이 아니고, 나쁜 코드의 악취를 풍기지만, 이렇게도 할 수 있습니다.

```swift
let magicBeings: Observable<MagicBeing> = summonFromMiddleEarth()

magicBeings
  .subscribe(onNext: { being in // Rx 모나드 탈출
    self.doSomething(being)
  })
  .disposed(by: disposeBag)

let kitten = globalParty(being, UIApplication.delegate.dataSomething.attendees)
kittens.on(.next(kitten)) // 결과를 rx로 다시 보내기

let kittens = BehaviorRelay(value: firstKitten) // Rx 모나드로 다시 돌아옴

kittens.asObservable()
  .map { kitten in
    return kitten.purr()
  }
```

이를 할 떼ㅐ마다 누군가는 어딘가에 이 코드를 작성할 것입니다.

```swift
kittens
  .subscribe(onNext: { kitten in 
    // do something with kitten
  })
  .disposed(by: disposeBag)
```

그러므로 이렇게 하지 마십시오.

## 플레이그라운드

#### Playgrounds

몇몇 오퍼레이터가 어떻게 작동하는지 정확하게 확신할 수 없다면, [플레이그라운드](https://github.com/ReactiveX/RxSwift/blob/master/Rx.playground)는 그것들의 행위를 묘사한 이미 준비된 작은 예제와 함께 거의 모든 오퍼레이터들을 포함합니다.

**플레이그라운드를 사용하려면 Rx.workspace를 열고 RxSwift-macOS 스킴을 빌드한 후 Rx.xcworkspace 트리 뷰에서 플레이그라운드를 여십시오.**

**플레이그라운드에서 예제의 결과를 보려면 `Assistant Editor` 를 여십시오. `View > Assistant Editor > Show Assistant Editor` 를 클릭하여 `Assistant Editor` 를 열 수 있습니다.**

## 에러 처리

#### Error handling

두 가지 에러 매커니즘이 있습니다.

### observable에서의 비동기 에러 처리 매커니즘

#### Asynchronous error handling mechanism in observables

에러 처리는 꽤 직관적입니다. 어떤 시퀀스가 에러와 함께 종료되었다면, 시퀀스에 의존하는 모든 것들은 에러와 함께 종료될 것입니다. 이는 보통 짧은 회로 논리입니다.

`catch` 오퍼레이터를 사용하여 observable의 실패로부터 회복할 수 있습니다. 세부적으로 회복을 명시할 수 있게 하는 다양한 오버로드가 있습니다.

에러가 발생한 시퀀스의 경우 재시도를 할 수 있게 하는 `retry` 오퍼레이터도 있습니다.

### 훅 및 기본 에러 처리

#### Hooks and Default error handling

RxSwift는 `onError` 핸들러를 제공하지 않은 경우에 디하여 기본 에러 처리 매커니즘을 제공하는 전역 Hook를 제공합니다.

필요하다면 `Hooks.defaultErrorHandler` 를 당신의 클로저에 설정하여 시스템에서 처리되지 않은 에러를 다루는 방법을 결정하십시오. 예를 들어 애널리틱스 시스템에 스택트레이스나 추적되지 않은 에러를 보낼 수 있습니다.

기본적으로 `Hooks.defaultErrorHandler` 는 `DEBUG` 모드에서는 받은 에러를 출력할 뿐이며, `RELEASE` 모드에서는 아무것도 하지 않습니다. 그러나 이 행위에 부가적인 구성을 추가할 수 있습니다.

세부적인 호출 스택 로깅을 가능하게 하려면 `Hooks.recordCallStackOnError` 플래그를 `true` 로 설정하십시오.

기본적으로 이는 `DEBUG` 모드에서는 현재 `Thread.callStackSymbols` 를 반환할 것이며, `RELEASE` 모드에서는 비어 있는 스택트레이스를 추적할 것입니다. 당신의 구현에 `Hooks.customCaptureSubscriptionCallStack` 을 오버라이딩하여 이 행위를 커스터마이징할 수 있을 것입니다.

## 컴파일 에러 디버깅

#### Debugging Compile Errors

우아한 RxSwift / RxCocoa 코드를 작성할 때, `Observable`의 타입을 추론하기 위해 컴파일러에 많이 의존할 것입니다. 이는 Swift가 놀라운 이유 중 하나이지만 때때로 혼란스럽게 할 수 있습니다.

```swift
images = word
  .filter { $0.containsString("important") }
  .flatMap { word in
    return self.api.loadFlickrFeed("karate")
      .catchError { error in
      return just(JSON(1))
    }
  }
```

컴파일러가 이 표현식의 어딘가에 에러가 있다고 보고한다면, 먼저 반환 타입을 명시하는 것을 제안합니다.

```swift
images = word
  .filter { s -> Bool in s.containsString("important") }
  .flatMap { word -> Observable<JSON> in
    return self.api.loadFlickrFeed("karate")
      .catchError { error -> Observable<JSON> in
      return just(JSON(1))
    }
  }
```

작동하지 않는다면, 에러를 알아낼 때까지 더 많은 곳에 타입을 명시해줄 수 있습니다.

```swift
images = word
  .filter { (s: String) -> Bool in s.containsString("important") }
  .flatMap { (word: String) -> Observable<JSON> in
    return self.api.loadFlickrFeed("karate")
      .catchError { (error: Error) -> Observable<JSON> in
      return just(JSON(1))
    }
  }
```

**먼저 클로저의 반환 타입과 인자를 명시하는 것을 제안합니다.**

보통 에러를 고친 후 코드를 다시 정리하기 위해 명시한 타입을 제거할 수 있습니다.

## 디버깅

#### Debugging

별도의 디버거를 사용하는 것도 유용하지만, 보통 `debug` 오퍼레이터를 사용하는 것이 더 능률적일 것입니다. `debug` 오퍼레이터는 표준 출력에 모든 이벤트를 출력할 것이며 그러한 이벤트에 레이블을 추가할 수 있습니다.

`debug` 는 정찰기처럼 행동합니다. 이를 사용하는 예제입니다.

```swift
let subscription = myInterval(.milliseconds(100))
  .debug("my probe")
  .map { e in
    return "This is simply \(e)"
  }
  .subscribe(onNext: { n in
    print(n)
  })

Thread.sleepForTimeInterval(0.5)

subscription.dispose()
```

다음을 출력할 것입니다.

```text
[my probe] subscribed
Subscribed
[my probe] -> Event next(Box(0))
This is simply 0
[my probe] -> Event next(Box(1))
This is simply 1
[my probe] -> Event next(Box(2))
This is simply 2
[my probe] -> Event next(Box(3))
This is simply 3
[my probe] -> Event next(Box(4))
This is simply 4
[my probe] dispose
Disposed
```

당신만의 `debug` 오퍼레이터를 쉽게 만들 수도 있습니다.

```swift
extension ObservableType {
  public func myDebug(identifier: String) -> Observable<Self.E> {
    return Observable.create { observer in
      print("subscribed \(identifier)")
      let subscription = self.subscribe { e in
        print("event \(identifier)  \(e)")
        switch e {
        case .next(let value):
          observer.on(.next(value))

        case .error(let error):
          observer.on(.error(error))

        case .completed:
          observer.on(.completed)
        }
      }
      return Disposables.create {
        print("disposing \(identifier)")
        subscription.dispose()
      }
    }
  }
 }
```

### 디버그 모드 활성화하기

#### Enabling Debug Mode

[`RxSwift.Resources` 사용하여 메모리 누수 디버깅](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#debugging-memory-leaks)하거나 [자동으로 모든 HTTP 요청을 로깅](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#logging-http-traffic)하기 위해 디버기 모드를 활성화해야 합니다.

디버그 모드를 활성화하려면 `TRACE_RESOURCES` 플래그가 RxSwift 타겟 빌드 설정의 *Other Swift Flags*에 추가되어야 합니다.

Cocoapods 및 Carthage에서 `TRACK_RESOURCES` 플래그를 설정하는 방법에 대한 더 많은 논의나 지시를 [#378](https://github.com/ReactiveX/RxSwift/issues/378)에서 확인하십시오.

## 메모리 누수 디버깅하기

#### Debugging memory leaks

디버그 모드에서 Rx는 전역 변수 `Resources.total` 에서 할당된 모든 리소스를 추적합니다.

어떠한 리소스 누수 탐지 로직을 원하는 경우, 가장 간단한 방법은 주기적으로 `RxSwift.Resources.total` 을 출력하는 것입니다.

```swift
/* add somewhere in
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil)
*/
_ = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
  .subscribe(onNext: { _ in
    print("Resource count \(RxSwift.Resources.total)")
  })
```

메모리 누수를 테스트하는 가장 눙률적인 방법은,

- 화면으로 내비게이션하여 사용하기
- 내비게이션 되돌리기
- 초기 리소스 카운트 관찰하기
- 화면으로 한번 더 내비게이션하여 사용하기
- 내비게이션 되돌리기
- 마지막 리소스 카운트 관찰하기

초기 및 마지막 리소스 카운트가 다르다면 어딘가에서 메모리 누수가 발생한 것일 것입니다.

두 번 내비게이션하는 이유는 첫 번째 내비게이션이 리소스의 지연 로딩을 강제하기 때문입니다.

## 키-값 옵저빙

#### KVO

KVO는 Objective-C 매커니즘입니다. 이는 타입 안전을 염두에 두고 설계돠지 않았음을 의미합니다. 이 프로젝트는 그러한 문제를 해결하려고 합니다.

이 라이브러리는 KVO를 지원하는 두 가지 방법을 내장하고 있습니다.

```swift
// KVO
extension Reactive where Base: NSObject {
  public func observe<E>(type: E.Type, _ keyPath: String, options: KeyValueObservingOptions, retainSelf: Bool = true) -> Observable<E?> {}
}

#if !DISABLE_SWIZZLING
// KVO
extension Reactive where Base: NSObject {
  public func observeWeakly<E>(type: E.Type, _ keyPath: String, options: KeyValueObservingOptions) -> Observable<E?> {}
}
#endif
```

`UIView`의 frame을 관찰하는 방뻐에 대한 예제입니다.

**주의 : UIKit은 KVO와 호환되지 않으나 동작할 것입니다.**

```swift
view
  .rx.observe(CGRect.self, "frame")
  .subscribe(onNext: { frame in 
    ...
  })
```

```swift
view
  .rx.observeWeakly(CGRect.self, "frame")
  .subscribe(onNext: { frame in
    ...
  })
```

### `rx.observe`

`rx.observe` 는 단지 KVO 매커니즘을 래핑한 것이므로 성능 기준에 더 잘 맞습니다. 하지만 사용 시나리오가 더욱 제한됩니다.

- 소유 그래프(`retainSelf = false`)에서 `self` 또는 그 조상들로부터 시작하는 경로를 관찰할 때 사용할 수 있습니다.
- 소유 그래프(`retainSelf = true`)에서 그 자손들로부터 시작하는 경로를 관찰할 때 사용할 수 있습니다.
- 경로는 `strong` 프로퍼티로만 구성되어야 합니다. 그렇지 않으면 해제 전에 KVO 옵저버의 등록을 해제하는 것을 하지 않음으로써 시스템에 충돌을 일으킬 위험을 안게 됩니다.

예)

```swift
self.rx.observe(CGRect.self, "view.frame", retainSelf: false)
```

### `rx.observeWeakly`

`rx.observeWeakly` 는 약한 참조의 경우 객체 해제를 처리해야 하므로 `rx.observe` 보다 다소 느립니다.

이는 `rx.observe` 를 사용할 수 있는 모든 케이스에서 사용할 수 있고, 추가적으로 다음의 경우에서도 사용 가능합니다.

- 관찰되는 타겟을 보유*retain*하지 않으므로, 소유 관계가 알려지지 않은 임의의 객체 그래프를 관찰하기 위해 사용될 수 있습니다.
- `weak` 프로퍼티를 관찰하기 위해 사용될 수 있습니다.

예)

```swift
someSuspiciousViewController.rx.observeWeakly(Bool.self, "behavingOk")
```

### 구조체 관찰

#### Observing structs

KVO는 Objective-C 매커니즘이므로 `NSValue` 에 깊게 의존합니다.

**RxCocoa는 `CGRect`, `CGSize`, `CGPoint` 구조체에 대한 KVO 관찰을 지원하도록 설계되었습니다.**

다른 구조체를 관찰하려면 직접 그러한 구조체들을 `NSValue` 에서 추출해내어야 합니다.

[여기](https://github.com/ReactiveX/RxSwift/blob/master/RxCocoa/Foundation/KVORepresentable+CoreGraphics.swift)에 `KVORepresentable` 프로토콜을 구현하여 다른 구조체들에 대하여 KVO 관찰 매커니즘과 `rx.observe*` 메소드를 확장하는 방법에 대한 예제가 있습니다.

## UI 레이어 팁

#### UI layer tips

`Observable` 이 UIKit 컨트롤과 바인딩될 때 UI 레이어에서 만족할 필요가 있는 것들이 있습니다.

### 스레딩

#### Threading

`Observable` 은 `MainScheduler`(UI 스레드)에 값을 보낼 필요가 있습니다. 이는 일반적인 UIKit / Cocoa의 요구사항일 뿐입니다.

당신의 API가 `MainScheduler` 에서 결과를 반환하도록 하는 것은 일반적으로 좋은 생각입니다. 백그라운드 스레드로부터 어떠한 것을 UI에 바인딩하려는 경우, **디버그 모드**에서 RxCocoa를 빌드하는 것은 보통 이에 대한 것을 알리기 위해 예외를 던질 것입니다.

이를 해결하기 위해 `observeOn(MainScheduler.instance)` 를 추가할 필요가 있습니다.

**URLSession 익스텐션은 기본적으로 `MainScheduler` 에 결과를 반환하지 않습니다.**

### 에러

#### Errors

UIKit 컨트롤에 실패를 바인딩할 수 없습니다. 이는 정의되지 않은 행위이기 때문입니다.

`Observable` 이 실패할 수 있다는 것을 알지 못한다면, `catchErrorJustReturn(valueThatIsReturnedWhenErrorHappens)` 를 사용하여 그것이 실패할 수 없다는 것을 확실히 해야 합니다. **하지만 에러가 발생한 후 기반 시퀀스는 여전히 종료될 것입니다.**

원하는 행위가 기반 시퀀스가 요소를 지속적으로 생산하는 것이라면, `retry` 오퍼레이터의 어떠한 버전이 필요할 것입니다.

### 구독 공유

#### Sharing subscription

일반적으로 UI 레이어에서 구독을 공유하기 원합니다. 같은 데이터를 여러 개의 UI 요소에 바인딩하기 위해 개별의 HTTP 호출을 하기 원치 않습니다.

다음의 것을 생각해 봅시다.

```swift
let searchResults = searchText
  .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
  .distinctUntilChanged()
  .flatMapLatest { query in
    API.getSearchResults(query)
      .retry(3)
      .startWith([]) // 새로운 검색 조건에 대하여 결과 초기화
      .catchErrorJustReturn([])
    }
  .share(replay: 1)    // <- `share` 오퍼레이터에 주목
```

당신이 일반적으로 원하는 것은 한 번 계산된 검색 결과를 공유하는 것입니다. 이것이 `share` 가 의미하는 것입니다.

**UI 레이어에서 변환 사슬*transformation chain*의 끝에 `share` 를 추가하는 것은 일반적으로 좋은 규칙입니다. 당신은 정말로 계산된 결과를 공유하기를 원하기 때문입니다. `searchResults` 를 여러 개의 UI 요소에 바인딩할 때 개별의 HTTP 연결을 발생시키는 것을 원하지 않습니다.**

**또한 `Drvier` 유닛에 대해 살펴보십시오. 이것은 그러한 `share` 호출을 명백하게 감싸, 요소들이 메인 UI 스레드에서 관찰되고 UI에 어떠한 에러도 바인딩되지 않는 것을 보장하기 위해 고안되었습니다.**

## HTTP 요청 만들기

#### Making HTTP requests

HTTP 요청을 만드는 것은 사람들이 시도하는 첫 번째 것 중 하나입니다.

먼저 완료될 필요가 있는 작업을 나타내는 `URLRequest` 객체를 만들 필요가 있습니다.

요청은 GET 요청인지, POST 요청인지, 요청 바디는 무엇인지, 쿼리 매개변수는 무엇인지 결정합니다.

아래는 간단한 GET 요청을 만드는 방법입니다.

```swift
let req = URLRequest(url: URL(string: "http://en.wikipedia.org/w/api.php?action=parse&page=Pizza&format=json"))
```

저 요청을 다른 observable들의 조합 바깥에서 실행하기 원한다면, 이것은 완료될 필요가 있는 것입니다.

```swift
let responseJSON = URLSession.shared.rx.json(request: req)

// 이 때에는 어떠한 요청도 수행되지 않을 것이다.
// `responseJSON`은 응답을 가져오는 방법에 대한 묘사일 뿐이다.


let cancelRequest = responseJSON
    // 이것이 요청을 발생시킬 것이다.
    .subscribe(onNext: { json in
        print(json)
    })

Thread.sleep(forTimeInterval: 3.0)

// 3초 후 요청을 취소하고 싶다면 다음을 호출하라.
cancelRequest.dispose()
```

**URLSession 익스텐션은 기본적으로 `MainScheduler` 에 결과를 반환하지 않습니다.**

응답에 대한 저수준 접근을 원한다면 다음을 사용할 수 있습니다.

```swift
URLSession.shared.rx.response(myURLRequest)
  .debug("my request") // 콘솔에 정보 출력
  .flatMap { (data: NSData, response: URLResponse) -> Observable<String> in
    if let response = response as? HTTPURLResponse {
      if 200 ..< 300 ~= response.statusCode {
        return just(transform(data))
      }
      else {
        return Observable.error(yourNSError)
      }
    }
    else {
      rxFatalError("response = nil")
      return Observable.error(yourNSError)
    }
  }
  .subscribe { event in
    print(event) // 에러가 발생하면 또한 콘솔에 에러를 출력
  }
```

### HTTP 트래픽 로깅

#### Logging HTTP traffic

디버그 모드에서 RxCocoa는 기본적으로 콘솔에 모든 HTTP 요청을 기록할 것입니다. 이 행위를 변경하고 싶다면 `Logging.URLRequests` 필터를 설정하십시오.

```swift
// read your own configuration
public struct Logging {
  public typealias LogURLRequest = (URLRequest) -> Bool

  public static var URLRequests: LogURLRequest =  { _ in
  #if DEBUG
    return true
  #else
    return false
  #endif
  }
}
```

## RxDataSources

이는 `UITableView` 와 `UICollectionView` 에 대하여 완전하게 함수적으로 리액티브한 데이터 소스를 구현하는 클래스의 집합입니다.

RxDataSources는 [여기](https://github.com/RxSwiftCommunity/RxDataSources) 있습니다.

[RxExample](https://github.com/ReactiveX/RxSwift/blob/master/RxExample) 프로젝트에 이들을 완전하게 함수적으로 사용하는 방법에 대해 나와 있습니다.