# Timeout

### 분류

Observable 유틸리티 연산자

### Observable 연산자 결정 트리

1. 오류가 발생했을 때 Observable이 옵저버에게 오류를 알려야 하며
2. 만약 항목이 배출되지 않은 상태에서 특정 시간이 경과했다면

---

소스 Observable을 미러링하지만, 특정 기간이 지난 후에도 어떠한 아이템도 배출되지 않았다면, 에러 알림을 발행한다.

---

`other` 인자에 특정 기간이 지난 이후 대체할 ObservableConvertibleType을 넘겨줄 수 있으며, 이 경우 onError 알림을 주는 대신 해당 Observable로 대체하여 작업을 수행하게 된다.

---

특정 기간이 지나도 아이템을 배출하지 않는다면 에러를 얻거나, 다른 Observable로 대체하여 작업을 수행하고 싶을 때 사용한다.

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

```swift
let subject = PublishSubject<Int>()
subject.timeout(0.3, other: Observable.from([1, 2, 3]), scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// completed
```

