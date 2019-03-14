# TakeLast

### 분류

Observable 필터링

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 마지막 몇 개의 항목들만 재배출해야 한다면

---

Observable이 배출하는 마지막 n개의 아이템만을 배출

---

### Take와의 차이

Take는 Observable이 배출하는 최초 n개의 아이템만을 배출한다.

TakeLast는 Observable이 배출하는 마지막 n개의 아이템만을 배출한다.

---

Observable이 배출하는 마지막 아이템 중 몇 개만을 취하고 싶을 때 사용한다.

```swift
Observable<Int>.from([1, 2, 3])
  .takeLast(2)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(2)
// next(3)
// completed
```

