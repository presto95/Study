# Single

### 분류

Observable 필터링

First의 변형

### Observable 연산자 결정 트리

1. Observable이 배출한 특정 항목을 조회해야 하는데
2. 배출된 항목이 단 하나이고 이것을 조회해야 한다면

---

RxSwift의 Single 오퍼레이터는 필터링보다는 블로킹 역할을 한다. Observable의 아이템 수가 하나 이상이면 첫 번째 아이템을 배출하고 에러를 발생시키며 종료되며, 아이템 수가 하나면 그 아이템을 배출하고 정상적으로 종료된다.

필터링을 위한 클로저를 넘겨줄 수 있다.

---

Observable의 첫 번째 아이템만을 취하는 것이 목적이라면, Single 대신 `take(1)` 이나 `elementAt(0)` 을 사용하는 것이 낫다.

---

```swift
let observable = Observable.from([1, 2, 3])
observable
  .single()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// error(Sequence contains more than one element.)

observable
  .single { $0 > 1 }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(2)
// error(Sequence contains more than one element.)

observable
  .single { $0 == 2 }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(2)
// completed
```

