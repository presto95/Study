# Scan

### 분류

Observable 변형

### Observable 연산자 결정 트리

1. Observable이 배출한 항목들을 변환한 후에 다시 배출해야 하는데
2. 앞에서 실행된 결과를 기반으로 항목을 변환한 후 배출해야 한다면

---

Observable이 배출하는 각각의 아이템에 순차적으로 함수를 적용하고, 각각의 연속 값을 배출한다.

---

Accumulator. 

### Reduce와의 차이

Reduce는 Observable이 배출하는 모든 아이템에 순차적으로 함수를 적용하고 최종 값만을 배출한다.

Scan은 Observable이 배출하는 모든 아이템에 순차적으로 함수를 적용하며, 함수가 적용된 값을 갖는 각각의 아이템을 순차적으로 배출한다.

```swift
Observable.from([1, 2, 3, 4, 5])
  .scan(0, accumulator: +)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(3)
// next(6)
// next(10)
// next(15)
// completed
```

