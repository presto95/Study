# SubscribeOn

### 분류

Observable 유틸리티 연산자

### Observable 연산자 결정 트리

1. 연산자가 특정 스케줄러 상에서 동작해야 한다면

---

Observable이 작동할 스케줄러를 명시

---

자주 사용되지는 않는다. 특정 스케줄러 상에서의 구독 및 구독 해지에 대한 사이드 이펙트만을 수행한다.

스케줄러에 관한 내용은 ObserveOn을 참고하자.

```swift
Observable.from([1, 2, 3])
  .subscribeOn(MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// completed
```

