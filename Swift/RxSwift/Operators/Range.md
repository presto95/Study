# Range

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 연속된 정수를 배출해야 한다면

---

특정 범위의 순차적 정수를 배출하는 Observable 생성

---

정수 타입에만 사용 가능하다. 부동 소수점 타입이나 문자열 타입에는 사용할 수 없다.

```swift
Observable.range(start: 0, count: 3)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// next(1)
// next(2)
// completed
```

