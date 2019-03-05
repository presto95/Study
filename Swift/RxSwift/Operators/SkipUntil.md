# SkipUntil

### 분류

조건 및 불리언 연산자

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 재배출할 항목들이 처음 몇 개 이후의 것들일 경우
3. 두 번째 Observable이 항목을 배출한 이후의 것들이라면

---

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

