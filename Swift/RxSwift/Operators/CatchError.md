# CatchError

### 분류

오류 처리 연산자

Catch의 변형

### Observable 연산자 결정 트리

1. 자연스럽게 Observable을 복구해야 하는데
2. 앞에서 발생한 오류 알림으로부터 복구해야 한다면

---

onError 알림이 발생한 경우 에러를 배출하는 대신 명시한 다른 동작을 취하도록 한다.

```swift
let observable = Observable<Int>.create { observer in
  let randomNumber = Int.random(in: 0...3)
  if randomNumber < 3 {
    observer.onError(NSError(domain: "", code: 0, userInfo: nil))
  } else {
    observer.onNext(randomNumber)
    observer.onCompleted()
  }
  return Disposables.create()
}

observable
  .do(onNext: { print("onNext: \($0)") },
      onError: { print("onError: \($0)") },
      onCompleted: { print("onCompleted") })
  .catchError { error in Observable.empty() }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// onError: Error Domain= Code=0 "(null)"
// completed
```

위의 경우 에러가 발생하면 `Observable.empty()` 의 동작을 취하도록 하였다.

에러가 발생하였고, 에러를 배출하는 대신 `.empty()`의 동작을 취하여 complete된 모습을 확인할 수 있다.

