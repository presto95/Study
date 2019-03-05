# DistinctUntilChanged

### 분류

Observable 필터링

Distinct의 변형

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 이미 배출된 항목과 동일한 것들을 제외시켜 재배출해야 하며
3. 만약 중복된 항목이 바로 연이어 배출된다면

---

Observable에 흐르는 어떠한 아이템이 직전의 아이템과 같으면 해당 아이템을 배출하지 않고, 직전의 아이템과 같지 않으면 해당 아이템을 배출한다.

---

클로저를 넘겨주어 아이템 간 동등 조건을 정의할 수 있다.

어떠한 상태를 구독하는 경우에 많이 사용한다.

ReactorKit을 사용할 때도 state를 바인딩하는 경우 자주 사용한다.

```swift
let subject = PublishSubject<Int>()
subject
  .distinctUntilChanged()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(2)
subject.onNext(3)
subject.onNext(1)

// next(1)
// next(2)
// next(3)
// next(1)
```

