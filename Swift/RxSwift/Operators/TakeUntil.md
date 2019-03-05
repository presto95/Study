# TakeUntil

### 분류

조건 및 불리언 연산자

### Observable 연산자 결정 트리

1. Observable의 특정 항목만 재배출해야 하는데
2. 마지막 항목 몇 개를 제외한 경우
3. 두 번째 Observable이 항목을 배출한 이후에 배출된 것들을 제외한 것이라면

---

두 번째 Observable이 아이템을 배출하거나 종료된 후 어떠한 Observable에서 배출된 아이템을 버리기

---

RxSwift에서 takeUntil은 두 가지 형태로 존재한다.

1. 주어진 Observable이 아이템을 배출하거나 종료될 때까지 리시버 Observable은 아이템을 배출한다. 주어진 Observable이 아이템을 배출하거나 종료되면 takeUntil 오퍼레이터는 미러링을 중단하고 스트림은 종료된다.
2. 주어진 Observable이 주어진 조건을 만족할 때까지 아이템을 배출하며, 조건을 만족한 아이템도 배출할지 배출하지 않을지에 대한 정보(TakeUntilBehavior)를 함께 넘겨준다. 조건에 만족할 때 종료되며, 조건을 만족하지 않아도 기존 Observable의 행동을 한 후 종료된다.
   - TakeUntilBehavior는 inclusive, exclusive 두 가지 케이스로 정의되어 있다. inclusive의 경우 조건을 만족하는 아이템도 배출하고 스트림을 종료시키며, exclusive의 경우 조건을 만족하는 아이템은 배출하지 않고 스트림을 종료시킨다.

---

1. 주어진 Observable이 아이템을 배출할 때까지만 리시버 Observable이 아이템을 배출해야 할 때 사용한다.
2. 아이템 배출을 중단할 조건이 있는 경우에 사용한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<Int>()
// 1
subject1.takeUntil(subject2)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
// 2
subject2.takeUntil(.exclusive) { $0 == 2 }
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject1.onNext(1)
subject1.onNext(2)
subject1.onNext(3)
subject2.onNext(1)
subject2.onNext(2)
subject1.onNext(4)

// 1
// next(1)
// next(2)
// next(3)
// completed

// 2
// next(1)
// completed
```

