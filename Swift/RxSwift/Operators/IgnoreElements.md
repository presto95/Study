# IgnoreElements

### 분류

Observable 필터링

### Observable 연산자 결정 트리

1. Observable이 배출하는 모든 객체를 무시하고 completed/error 알림만 전달해야 한다면

---

Observable이 배출하는 어떠한 아이템도 배출하지 않으며, 그것의 종료 알림(onError / onCompleted)만 배출한다.

---

onNext 이벤트를 배출하지 않는 것을 보장한다.

Observable이 배출하는 아이템에는 관심이 없으나 그 종료 알림은 받고 싶을 때 사용한다.

```swift
Observable.from([1, 2, 3])
  .ignoreElements()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// completed
```

