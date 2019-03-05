# Reduce

### 분류

수학 및 집계 연산자

---

Observable이 배출하는 각 아이템에 함수를 적용하여 최종 값을 배출

---

Swift의 reduce와 동작이 비슷하다고 생각해도 되겠다.

```swift
Observable.from([1, 2, 3])
  .reduce(0, { $0 + $1 })
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(6)
// completed
```

