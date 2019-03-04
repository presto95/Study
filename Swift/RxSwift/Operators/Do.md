# Do

**Observable 유틸리티 연산자**

다양한 Observable의 생명 주기 이벤트에서 취할 액션을 등록

---

Do를 통하여 액션을 등록할 수 있는 이벤트는 onNext, onError, onCompleted, onSubscribe, onSubscribed, onDispose 총 6개이다.

해당 이벤트가 발생했을 때의 콜백을 등록하는 것이다.

---

### Subscribe와의 차이

반환형부터 다르다!

- Subscribe 오퍼레이터의 반환형은 Disposable으로, Subscribe를 통해 Observable을 구독하게 되면 이후에는 오퍼레이터를 통하여 변환할 수 없다.
- Do 오퍼레이터의 반환형은 Observable으로, Do 직전까지 변환된 Observable의 아이템이 Observable의 생명 주기 이벤트에서 넘겨지게 된다.

---

Observable 구독으로 인해 어떠한 이벤트가 발생할 때 해당 라인까지 변환된 Observable로 수행할 동작을 등록하기 위해 사용한다.

```swift
Observable.from([1, 2, 3])
  .do(onNext: { value in
    print("Next: \(value)")
  })
  .map { $0 * 2 }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// Next: 1
// next(2)
// Next: 2
// next(4)
// Next: 3
// next(6)
// completed
```

