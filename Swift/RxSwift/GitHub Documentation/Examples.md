# 예제

## 반응형 값

#### Reactive values

먼저 명령형 코드로 시작합시다. 이 예제의 목적은 어떤 조건이 만족되면 `c` 식별자를 `a` 와 `b` 로부터 계산된 값에 바인딩하는 것입니다.

`c` 값을 계산하는 명령형 코드입니다.

```swift
// 표준 명령형 코드
var c: String
var a = 1       // `a` 에 `1` 값을 한 번 할당할 뿐
var b = 2       // `b` 에 `2` 값을 한 번 할당할 뿐

if a + b >= 0 {
    c = "\(a + b) is positive" // `c` 에 값을 한 번 할당할 뿐
}
```

이제 `c` 값은 `3 is positive` 입니다. 그러나 `a` 의 값이 `4` 로 변화한다면, `c` 는 여전히 이전의 값을 가지고 있을 것입니다.

```swift
a = 4     // `c` 는 여전히 `3 is positive` 와 같으며, 좋지 않다.
          // 4 + 2 = 6 이므로 `c` 가 `6 is positive` 와 같게 되는 것을 원한다.
```

이는 기대한 행위가 아닙니다.

RxSwift를 사용하여 개선한 로직입니다.

```swift
let a /*: Observable<Int>*/ = BehaviorRelay(value: 1)   // a = 1
let b /*: Observable<Int>*/ = BehaviorRelay(value: 2)   // b = 2

// `+` 를 사용하여 `a` 와 `b` relay의 최신 값을 조합
let c = Observable.combineLatest(a, b) { $0 + $1 }
	.filter { $0 >= 0 }               // `a + b >= 0` 이 참이면 `a + b` 는 map 오퍼레이터로 전달됨
	.map { "\($0) is positive" }      // `a + b` 를 "\(a + b) is positive" 로 매핑

// 초기값은 a = 1, b = 2
// 1 + 2 = 3 이고 이는 0 이상이므로, `c` 는 초기에 "3 is positive" 와 같다.

// Rx의 `Observable` `c` 에서 값을 밀어내기 위해 `c` 로부터의 값을 구독한다.
// `subscribe(onNext:)` 은 `c` 의 다음 (새로운) 값을 구독한다는 의미다.
// 또한 초기값 "3 is positive"를 포함한다.
c.subscribe(onNext: { print($0) })          // 출력: "3 is positive"

// 이제 `a` 의 값을 증가시키자.
a.accept(4)                                   // 출력: 6 is positive
// 최신 값 `4` 와 `2` 의 합은 이제 `6` 이다.
// 이는 `>= 0` 이기 때문에 `map` 오퍼레이터는 "6 is positive" 를 생산한다.
// 그리고 그 결과는 `c` 에 "할당"된다.
// `c` 의 값이 변화했기 때문에 `{ print($0) }` 이 호출될 것이며,
// "6 is positive"가 출력될 것이다.

// 이제 `b` 의 값을 변경하자.
b.accept(-8)                                 // 아무것도 출력하지 않음
// 최신 값의 합 `4 + (-8)`은 `-4` 이다.
// 이것은 `>= 0` 이 아니므로, `map` 은 실행되지 않는다.
// 이는 `c` 가 여전히 "6 is positive" 를 담는다는 것을 의미한다.
// `c` 가 갱신되지 않았기 때문에, 새로운 "next" 값은 생산되지 않으며,
// `{ print($0) }` 은 호출되지 않을 것이다.
```

## 간단한 UI 바인딩

#### Simple UI Bindings

- Relay로 바인딩하는 대신, `rx.text` 프로퍼티를 사용하여 `UITextField` 의 값을 바인딩합시다.
- 다음으로 `String` 을 `Int` 로 `map` 하고 비동기 API를 사용하여 숫자가 소수인지 결정합시다.
- 비동기 호출이 완료되기 전에 텍스트가 변경된다면, 새로운 비동기 호출은 `concat` 을 통하여 교체될 것입니다.
- 결과를 `UILabel` 에 바인딩합니다.

```swift
let subscription/*: Disposable */ = primeTextField.rx.text      // Observable<String> 타입
  .map { WolframAlphaIsPrime(Int($0) ?? 0) }                    // Observable<Observable<Prime>> 타입
  .concat()                                                     // Observable<Prime> 타입
  .map { "number \($0.n) is prime? \($0.isPrime)" }             // Observable<String> 타입
  .bind(to: resultLabel.rx.text)                                // 모두 바인딩 해제하기 위해 사용될 수 있는 Disposable 반환

// 서보 호출이 완료된 후 `resultLabel.text` 를 "number 43 is prime? true"로 설정할 것이다.
// 이것들은 RxCocoa가 내부적으로 관찰하는 UIKit 이벤트이므로 직접 컨트롤 이벤트를 트리거한다.
primeTextField.text = "43"
primeTextField.sendActions(for: .editingDidEnd)

// ...

// 모두 바인딩 해제하기 위해 호출한다.
subscription.dispose()
```

이 예제에서 사용된 모든 오퍼레이터는 첫 번째 예제에서 relay와 함께 사용된 것과 같습니다. 특별한 것이 없습니다.

## 입력 유효성 검사 자동화

#### Automatic input validation

Rx가 처음이라면 다음의 예제는 처음에는 약간 버거울 수 있습니다. 그러나 RxSwift 코드가 실제 세계에서 보는 방법을 보여주기 위한 것입니다.

이 예제는 프로그레스 알림과 함께 복잡한 비동기 UI 유효성 검사 로직을 포함합니다. 모든 작업은 `disposeBag` 이 할당 해제되는 시점에 취소됩니다.

```swift
enum Availability {
    case available(message: String)
    case taken(message: String)
    case invalid(message: String)
    case pending(message: String)

    var message: String {
        switch self {
        case .available(let message),
             .taken(let message),
             .invalid(let message),
             .pending(let message): 

             return message
        }
    }
}

// 직접 UI 컨트롤 이벤트를 바인딩
// 사용자 이름 소스로 `usernameOutlet` 의 사용자 이름 사용
self.usernameOutlet.rx.text
  .map { username -> Observable<Availability> in

    // 동기적 유효성 검사. 특별한 것 없음
    guard let username = username, !username.isEmpty else {
      // 동기 결과를 만들기 위한 편의.
      // 같은 메소드 내부에 동기 코드와 비동기 코드가 혼합되어 있다면, 즉시 해결되는 비동기 결과를 만들 것이다.
      return Observable.just(.invalid(message: "Username can't be empty."))
    }

    // ...

    // UI는 비동기 작업이 진행 중일 때 어떠한 상태를 보여줘야 할 것이다.
    let loadingValue = Availability.pending(message: "Checking availability ...")

    // 사용자 이름이 이미 존재하는지 확인하기 위해 서버 호출을 진행할 것이다.
    // `Observable<Bool>` 타입.
    return API.usernameAvailable(username)
      .map { available in
        if available {
          return .available(message: "Username available")
        }
        else {
          return .taken(message: "Username already taken")
        }
      }
      // 서버가 응답할 때까지 `loadingValue` 사용
      .startWith(loadingValue)
  }
  // `Observable<Observable<Availability>>` 타입을 갖게 되므로,
  // 어떻게든 이를 `Observable<Availability>` 로 간단하게 반환시킬 필요가 있다.
  // 두 번째 예제에서 `concat` 오퍼레이터를 사용할 수 있겠으나,
  // 새로운 사용자 이름이 제공되면 대기 중인 비동기 작업을 취소하기를 정말로 원한다.
  // 이것이 `switchLatest` 가 하는 것이다.
  .switchLatest()
  // 이제 어떻게든 UI에 그것을 바인딩해야 할 필요가 있다.
  // `subscribe(onNext:)` 를 사용하여 할 수 있다.
  // `Observable` 체인의 끝에 위치한다.
  .subscribe(onNext: { [weak self] validity in
    self?.errorLabel.textColor = validationColor(validity)
    self?.errorLabel.text = validity.message
  })
  // 이것은 모두 바인딩 해제하고 대기 중인 비동기 작업을 취소할 수 있는 `Disposable` 객체를 생산할 것이다.
  // 이를 직접 하는 것은 지루하므로, 뷰 컨트롤러가 메모리에서 해제될 때 자동으로 모든 것을 dispose하도록 하자.
  .disposed(by: disposeBag)
```
이것보다 더 간단해질 수 없습니다. 이 레포지토리에 [더 많은 예제](https://github.com/ReactiveX/RxSwift/blob/master/RxExample)가 있으므로 그것을 참고해 주십시오.

Rx를 MVVM 패턴의 또는 그 밖의 컨텍스트에서 사용하는 예제를 포함하고 있습니다.
