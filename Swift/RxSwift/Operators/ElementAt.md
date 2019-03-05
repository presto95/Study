# ElementAt

### 분류

Observable 필터링

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 몇 번째 위치한 항목만 재배출해야 한다면

---

Observable이 배출한 n번째 아이템만을 배출함

---

인자는 0부터 시작한다.

n번째 아이템을 배출하고 종료한다.

```swift
Observable.just([1, 2, 3])
	.elementAt(1)
	.subscribe { print($0) }
	.disposed(by: disposeBag)

// next(2)
// completed
```

