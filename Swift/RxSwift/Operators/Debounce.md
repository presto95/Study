# Debounce

### 분류

Observable 필터링

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 특정 시간이 지나고 나서 배출된 항목들만 재배출해야 한다면

---

특정 기간 동안 또다른 아이템을 배출하지 않았을 때만 Observable이 아이템을 배출

---

Observable이 어떠한 아이템을 배출하고, 특정 기간 동안 아이템을 배출하지 않았을 때 해당 아이템을 배출한다.

특정 기간 내에 Observable이 계속 아이템을 배출한다면, 결과적으로 어떠한 아이템도 배출되지 않는다.

---

소스 Observable이 전에 배출된 아이템을 따라서 너무 빠르게 아이템을 배출할 때, 이를 걸러내기 위해 사용한다.

```swift
let subject = PublishSubject<Int>()
subject
  .debounce(0.5, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
Timer.scheduledTimer(withTimeInterval: 1, repeats: false) { _ in
  subject.onNext(4)
}

// next(3)
// next(4)
```

