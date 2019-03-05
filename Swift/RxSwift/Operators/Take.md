# Take

### 분류

Observable 필터링

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 처음 몇 개의 항목들만 재배출해야 한다면

---

Observable이 배출한 최초의 n개의 아이템만을 배출

---

최초의 n개의 아이템을 배출하고 종료된다.

```swift
Observable.from[1, 2, 3]
  .take(2)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// completed
```

