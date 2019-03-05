# CatchErrorJustReturn

**오류 처리 연산자**

**Catch의 변형**

---

에러가 발생하면, 그 대신 `Observable.just(_:)`의 동작을 취하게 한다.

에러를 처리하는 가장 편리한 방식인 것 같다.

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
  .catchErrorJustReturn(1000)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// onError: Error Domain= Code=0 "(null)"
// next(1000)
// completed
```