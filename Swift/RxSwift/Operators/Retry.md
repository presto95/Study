# Retry

### 분류

오류 처리 연산자

### Observable 연산자 결정 트리

1. 자연스럽게 Observable을 복구해야 하는데
2. 앞에서 발생한 오류 알림으로부터 복구해야 하며
3. 이전 Observable에 재구독을 시도해야 한다면

---

소스 Observable이 에러를 배출한다면, 이것이 에러 없이 완료되기를 바라며 다시 구독함

---

Retry 오퍼레이터는 onError 알림에 반응한다. 이 에러를 observer에게 넘겨주는 대신, 해당 시퀀스가 에러 없이 완료될 기회를 제공하기 위해 소스 Observable을 다시 구독한다.

Retry 오퍼레이터는 결국에 에러로 종료되더라도 항상 onNext 알림을 넘겨준다. 그렇기 때문에 중복 아이템을 배출할 가능성이 있다.

---

RxSwift에서는 두 가지 형태의 Retry가 있다.

1. 인자가 없는 Retry (`retry()`)
   - 성공할 때까지 재시도한다. 그렇기 때문에 무한한 시퀀스를 만들어낼 가능성이 있다.
2. 재시도 횟수를 명시하는 Retry (`retry(_:)`
   - 인자에 명시된 횟수만큼 재시도한다. `retry(2)`라고 해야 에러가 발생 시 한 번 재시도하는 것이다.
   - 인자는 `maxAttemptCount`로, 최대 재시도 횟수가 아닌 최대 시도 횟수를 나타낸다.

---

어떠한 작업(네트워크 호출 등)에서 에러가 발생했을 때 이것을 다시 시도하기 위해 사용한다.

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
  .retry(3)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// onError: Error Domain= Code=0 "(null)"
// onError: Error Domain= Code=0 "(null)"
// onNext: 3
// next(3)
// onCompleted
// completed
```

위의 코드는 0부터 3까지의 난수를 발생시켜 3 미만이 나오면 onError, 3이 나오면 onNext와 onCompleted 이벤트를 배출하는 Observable을 만들고 구독하였으며, 에러가 발생하면 두 번 다시 구독하게 한 것이다.

결과는 매번 다르게 나올 것이다.

이 경우 세 번째 시도에서 onNext 이벤트가 발생하였다.