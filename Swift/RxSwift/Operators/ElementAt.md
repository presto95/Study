# ElementAt

**Observable 필터링**

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

