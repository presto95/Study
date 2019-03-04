# SkipUntil

**조건 및 불리언 연산자**

두 번째 Observable이 아이템을 배출할 때까지 어떠한 Observable이 배출하는 아이템을 버림

---

주어진 Observable이 아이템을 배출할 때까지 리시버 Observable이 배출하는 아이템을 무시하고 싶을 때 사용한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<Int>()
subject1.skipUntil(subject2)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

subject1.onNext(1)
subject1.onNext(2)
subject2.onNext(1)
subject1.onNext(3)
subject2.onNext(2)
subject1.onNext(4)

// next(3)
// next(4)
```

