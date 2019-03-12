# Timer

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 지정된 시간 이후에 항목을 배출해야 한다면

---

주어진 딜레이 이후에 특정 아이템을 배출하는 Observable 생성

---

`timer(_:period:scheduler)` 의 형태이며, `period` 는 기본값이다.

`period` 가 기본값인 경우 하나의 아이템을 배출하고 종료된다.

`period` 가 기본값이 아닌 경우 주어진 시간 간격으로 아이템을 무한하게 배출하며 종료되지 않는다.

---

일정 시간 간격으로 아이템을 배출하고 싶을 때 사용한다.

```swift
Observable<Int>.timer(0.3, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// completed
```

```swift
Observable<Int>.timer(0.3, period: 0.3, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// next(1)
// next(2)
// ...
// 0.3초 간격으로 순차적 아이템 무한하게 배출
```

두 코드 모두 0.3초 이후부터 아이템을 배출하기 시작한다.