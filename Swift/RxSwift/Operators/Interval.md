# Interval

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 연속된 정수를 배출해야 하며
2. 특정 시간 간격별로 항목을 배출해야 한다면

---

주어진 시간 간격별로 정수 시퀀스를 배출하는 Observable 생성

---

정수 시퀀스를 배출하므로 제네릭 타입에 정수형 타입을 명시해주어야 사용 가능하다.

기본적으로 종료되지 않은 Observable을 생성하며, 0부터 시작하여 무한한 정수 시퀀스 아이템을 배출하게 된다.

```swift
Observable<Int>
  .interval(0.3, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// next(1)
// next(2)
// ...
// 종료되지 않고 무한하게 아이템을 배출함
```

```swift
Observable<Int>
  .interval(0.3, scheduler: MainScheduler.instance)
  .take(3)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// next(1)
// next(2)
// completed
```

