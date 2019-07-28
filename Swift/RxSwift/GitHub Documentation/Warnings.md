# 주의

#### Warnings

## 사용되지 않은 disposable (unused-disposable)

이는 `Disposable` 을 반환하는 `subscribe*`, `bind*`, `drive*` 패밀리에 유효합니다.

이것처럼 하면 경고를 받을 것입니다.

```swift
let xs: Observable<E> ...

xs
  .filter { ... }
  .map { ... }
  .switchLatest()
  .subscribe(onNext: {
    ...
  }, onError: {
    ...
  })
```

`subscribe` 함수는 연산을 취소하고 리소스를 해제하기 위해 사용될 수 있는 구독 `Disposable` 을 반환합니다. 그러나 그것을 사용하지 않는 것 (dispose하지 않는 것) 은 에러의 결과를 가져올 것입니다.

이 유려한 호출들을 종료하는 선호되는 방법은 `DisposeBag` 을 사용하는 것입니다. `disposed(by: disposeBag)` 에 호출을 체이닝할 수도 있고, bag에 직접 disposable을 추가할 수도 있습니다.

```swift
let xs: Observable<E> ....
let disposeBag = DisposeBag()

xs
  .filter { ... }
  .map { ... }
  .switchLatest()
  .subscribe(onNext: {
    ...
  }, onError: {
    ...
  })
  .disposed(by: disposeBag) // <--- note `.disposed(by:)`
```

`disposeBag` 이 메모리에서 해제될 때, 그것이 포함하는 disposable들 또한 자동으로 dispose될 것입니다.

`xs` 가 `Completed` 또는 `Erorr` 메세지로 예측 가능한 방법으로 종료된 경우, `Disposable` 구독을 처리하지 않는 것은 어떠한 리소스의 누수를 발생시키지 않을 것입니다. 하지만 이러한 경우에도 dispose bag을 사용하는 것이 구독의 disposable을 다루는 선호되는 방법입니다. 이는 요소 연산이 항상 예측 가능한 순간에 종료되는 것을 보장하고, `xs` 의 구현이 변경되더라도 리소스가 적절하게 처리되므로 코드를 강력하고 미래의 증거로 만듭니다.

구독과 리소스가 어떠한 객체의 라이프타임에 묶여 있도록 하는 또다른 방법은 `takeUntil` 오퍼레이터를 사용하는 것입니다.

```swift
let xs: Observable<E> ....
let someObject: NSObject  ...

_ = xs
  .filter { ... }
  .map { ... }
  .switchLatest()
  .takeUntil(someObject.deallocated) // <-- note the `takeUntil` operator
  .subscribe(onNext: {
    ...
  }, onError: {
    ...
  })
```

구독 `Disposable` 을 무시하는 것이 바람직한 행위라면, 이는 컴파일러 경고를 침묵시키는 방법이 됩니다.

```swift
let xs: Observable<E> ....

_ = xs // <-- note the underscore
  .filter { ... }
  .map { ... }
  .switchLatest()
  .subscribe(onNext: {
    ...
  }, onError: {
    ...
  })
```

## 사용되지 않는 observable 시퀀스 (unused-observable)

다음과 같이 하면 경고를 받을 것입니다.

```swift
let xs: Observable<E> ....

xs
  .filter { ... }
  .map { ... }
```

이 코드는 `xs` 시퀀스로부터 필터링되고 매핑된 observable 시퀀스를 정의하지만 이후 결과를 무시합니다.

이 코드는 단지 observable 시퀀스를 정의할 뿐이고 그것을 무시하므로, 실제로는 아무것도 하지 않습니다.

당신이 의도한 것은 아마 observable 시퀀스의 정의를 저장하고 나중에 그것을 사용하거나,

```swift
let xs: Observable<E> ....

let ys = xs // <--- names definition as `ys`
  .filter { ... }
  .map { ... }
```

정의에 기반하여 연산을 시작하는 것이었을 것입니다.

```swift
let xs: Observable<E> ....
let disposeBag = DisposeBag()

xs
  .filter { ... }
  .map { ... }
  .subscribe(onNext: { nextElement in  // <-- note the `subscribe*` method
    // use the element
    print(nextElement)
  })
  .disposed(by: disposeBag)
```