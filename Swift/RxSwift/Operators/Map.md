# Map

**Observable 변환**

Observable이 배출하는 각 아이템에 어떠한 함수를 적용하여 아이템을 변환

---

Swift의 map과 비슷하다고 생각하면 되겠다.

```swift
Observable.from([1, 2, 3])
  .map { "\($0)" }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(10)
// next(20)
// next(30)
// completed
```

