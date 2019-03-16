# Zip

### 분류

Observable 결합

### Observable 연산자 결정 트리

1. 다른 Observable을 결합시켜 새로운 Observable을 생성해야 한다
2. 생성하고 싶은 Observable은, 두 개 이상의 Observable이 가진 항목들을 순서대로 결합시켜 새로운 항목을 배출해야 하는데
3. 각각의 Observable이 항목을 배출할 때마다 그 항목들을 결합시켜 배출해야 한다면

---

특정 함수를 거쳐서, 다수의 Observable이 배출하는 것을 함께 결합하고, 그 함수의 결과에 기반한 각각의 결합에 대한 단일 아이템을 배출

---

### CombineLatest와의 차이

CombineLatests는 각각의 소스 Observable이 최초로 아이템을 배출한 이후, 소스 Observable 중 하나라도 아이템을 배출하면 모든 소스 Observable이 마지막으로 배출한 아이템을 결합하여 배출한다.

Zip은 각각의 소스 Observable 모두가 아이템을 배출해야 그 아이템을 결합하여 배출한다. 각 아이템은 서로 시간적으로 쌍이 맞아야 한다.

---

Observable을 결합하고 싶은데, 각각의 Observable이 배출하는 아이템의 쌍을 맞추어야 할 때 사용한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<String>()
Observable.zip(subject1, subject2) { "\($0) | \($1)" }
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject1.onNext(1)
subject2.onNext("A")
subject1.onNext(2)
subject2.onNext("B")
subject2.onNext("C")
subject2.onNext("D")
subject1.onNext(3)
subject1.onNext(4)

// next(1 | A)
// next(2 | B)
// next(3 | C)
// next(4 | D)
```

