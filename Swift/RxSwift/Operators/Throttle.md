# Throttle

**Observable 필터링**

**Debounce의 변형**

---

Observable이 어떠한 아이템을 최초로 배출한 후, 특정 기간 동안 배출된 아이템 중 가장 최신에 배출된 것만을 배출한다.

---

### Debounce와의 차이

Debounce는 Observable이 아이템을 배출할 때마다 스톱워치를 돌려서, 스톱워치가 끝날 때까지 다른 아이템을 배출하지 않으면 해당 아이템을 배출하는 느낌.

Throttle은 Observable이 아이템을 배출하면 그 때부터 스톱워치를 돌려서, 스톱워치가 동작하는 동안 배출된 아이템 중 가장 최신에 배출된 것만을 배출한다. 스톱워치는 타이머가 끝나면 바로 다시 동작한다.

Throttle은 특정 기간 마다 아이템을 배출할 수도 있고 배출하지 않을 수도 있으며, 특정 기간 내에는 아이템을 배출하지 않는 것을 보장한다.

---

특정 기간 내에 두 개 이상의 아이템이 배출되지 않는 것을 보장한다.

특정 기간 내에 여러 변화가 일어났을 때, 마지막 변화만을 취하고 싶을 때 사용한다.

```swift
let subject = PublishSubject<Int>()
subject
  .throttle(0.5, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
Timer.scheduledTimer(withTimeInterval: 1, repeats: false) { _ in
  subject.onNext(4)
}

// next(1)
// next(3)
// next(4)
```

