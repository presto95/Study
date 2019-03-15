# Window

### 분류

Observable 변형

### Observable 연산자 결정 트리

1. 하나의 Observable을 여러 Observable로 나누어야 한다

---

Observable이 배출하는 아이템들을 Observable window에 주기적으로 재분할하여, 아이템들을 배출하는 대신 이 window들을 한번에 배출

---

### Buffer와의 차이

Buffer는 아이템들을 모아서 한번에 배출한다.

Window는 아이템을 모아 그것들이 흐르는 Observable을 각각 생성하며, 그 Observable은 모아진 아이템을 모두 배출한 후에는 종료된다.

---

```swift
Observable<Int>.interval(0.2, scheduler: MainScheduler.instance)
  .window(timeSpan: 2, count: 5, scheduler: MainScheduler.instance)
  .subscribe { print($0.element?.subscribe { print($0) }.disposed(by: self.disposeBag)) }
  .disposed(by: disposeBag)

// Optional(())
// next(0)
// next(1)
// next(2)
// next(3)
// next(4)
// completed
// Optional(())
// next(5)
// next(6)
// next(7)
// next(8)
// next(9)
// completed
// ...
/// 위처럼 반복
```

소스 Observable은 0.2초마다 0부터 연속적으로 정수를 배출하고, window 오퍼레이터를 통해 다섯 개의 아이템을 모아 Observable을 각각 배출하도록 하였다.

하나의 Observable이 생성될 때 그 Observable을 구독하게 하였으며, 0.2초마다 아이템을 배출하며 모든 아이템을 배출 완료하고 종료되는 것을 확인할 수 있다.

