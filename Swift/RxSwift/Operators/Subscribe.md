# Subscribe

### 분류

Observable 변환

---

Observable로부터의 배출과 알림에 따라 동작

---

observer를 Observable과 연결하는 역할을 하는 오퍼레이터.

Observable이 배출하는 아이템(onNext)나 에러, 완료 알림(onError, onCompleted)을 observer가 받기 위해서는 일단 Subscribe 해야 한다.

onNext, onError, onCompleted 세 가지 이벤트를 받게 된다. 이는 열거형으로 정의되어 있다.

---

"cold" Observable은 observer가 그것을 구독하기 전까지는 아이템 배출을 시작하지 않는다.

"hot" Observable은 아무 때나 아이템을 배출하며, 그렇기 때문에 observer가 그것을 구독하는 시점 이전에 Observable이 배출하는 아이템을 놓칠 수 있다.

---

기본이 되는 오퍼레이터. Observable을 구독하기 위해 사용한다.

```swift
Observable<Int>
  .just(1)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// completed
```

