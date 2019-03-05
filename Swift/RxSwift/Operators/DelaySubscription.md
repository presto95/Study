# DelaySubscription

### 분류

Observable 유틸리티 연산자

Delay의 변형

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 항목 배출이 시작된 이후에 얼마 동안 구독을 지연시켜야 한다면

---

### Delay와의 차이

Delay는 아이템 배출을 지연시킨다.

DelaySubscription은 최초 아이템 배출시 구독을 지연시킨다.

---

아이템 배출 시작 이후 구독을 지연시키고 싶을 때 사용한다.

```swift
let subject = PublishSubject<Int>()
subject
  .delaySubscription(0.5, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject.onNext(1)
Timer.scheduledTimer(withTimeInterval: 1, repeats: false) { _ in
  subject.onNext(2)
  subject.onCompleted()
}

// next(2)
// completed
```

onNext(1) 이벤트 발생 이후 구독이 0.5초 지연되었기 때문에 1초 후 발생한 onNext(2) 이벤트는 Subject 구독에 의해 알림을 받게 된다.