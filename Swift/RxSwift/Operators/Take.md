# Take

**Observable 필터링**

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

