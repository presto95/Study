# Filter

**Observable 필터링**

Observable의 아이템 중 주어진 테스트를 통과하는 아이템들만 배출

---

Swift의 filter와 비슷하다고 생각하면 될 것 같다.

```swift
Observable.from([1, 2, 3])
  .filter { $0 % 2 == 0 }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(2)
// completed
```

