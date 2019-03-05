# WithLatestFrom

### 분류

Observable 결합

CombineLatest의 변형

---

소스 Observable에 대하여, 주어진 Observable이 아이템을 배출한 이후 소스 Observable이 아이템을 배출할 때, 각 스트림이 마지막으로 배출한 아이템을 특정 함수를 통해 결합하고, 그 결과를 배출한다.

특정 함수는 주어질 수도, 주어지지 않을 수도 있다. 주어지지 않았다면, 결합한 Observable이 아이템을 배출할 때 주어진 Observable의 아이템을 배출한다.

---

### CombineLatest와의 차이

CombineLatest는 여러 개의 Observable을 결합하여 하나의 Observable을 만들며, 각 Observable이 하나의 아이템을 배출한 이후, 어떠한 Observable이 값을 배출할 때 각 Observable이 마지막으로 배출한 아이템과 특정 함수를 통해 결합하고, 그 결과를 배출한다.

WithLatestFrom은 소스 Observable을 주어진 Observable과 결합하여 하나의 Observable을 만들며, 주어진 Observable이 하나의 아이템을 배출한 이후, 소스 Observable이 값을 배출할 때 주어진 Observable이 마지막으로 배출한 아이템과 특정 함수를 통해 결합하고, 그 결과를 배출한다.

---

소스 Observable이 아이템을 배출할 때 주어진 Observable이 마지막으로 배출한 아이템을 배출하고 싶을 때, 또는 각각의 아이템을 특정 함수를 통해 결합하여 배출하고 싶을 때 사용한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<String>()
let withLatestFrom1 = subject1.withLatestFrom(subject2)
let withLatestFrom2 = subject1.withLatestFrom(subject2) { "\($0)\($1)" }
withLatestFrom1
  .subscribe { print($0) }
  .disposed(by: disposeBag)
withLatestFrom2
  .subscribe { print($0) }
  .disposed(by: disposeBag)

subject1.onNext(1)
subject1.onNext(2)
subject2.onNext("first")
subject1.onNext(3)
subject2.onNext("second")
subject2.onNext("third")

// next(first)
// next(3first)
```

