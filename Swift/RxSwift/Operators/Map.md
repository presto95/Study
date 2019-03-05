# Map

### 분류

Observable 변환

### Observable 연산자 결정 트리

1. Observable이 배출한 항목들을 변환한 후에 다시 배출해야 하는데
2. 함수와 함께 항목을 한번에 하나씩 변환 후 배출해야 한다면

---

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

