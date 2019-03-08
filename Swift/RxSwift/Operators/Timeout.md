# Timeout

### 분류

Observable 유틸리티 연산자

### Observable 연산자 결정 트리

1. 오류가 발생했을 때 Observable이 옵저버에게 오류를 알려야 하며
2. 만약 항목이 배출되지 않은 상태에서 특정 시간이 경과했다면

---

소스 Observable을 미러링하지만, 특정 기간이 지난 후에도 어떠한 아이템도 배출되지 않았다면, 에러 알림을 발행한다.

---

주어진 기간 동안 Observable이 아이템을 배출하지 않는다면 onError 알림을 주고 종료된다.

```swift
let subject = PublishSubject<Int>()
subject
  .timeout(0.5, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// error(Sequence timeout.)
```

```swift
let subject = PublishSubject<Int>()
subject
  .timeout(0.5, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

subject.onNext(1)

// next(1)
// error(Sequence timeout.)
```

