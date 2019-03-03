# CombineLatest

**Observable 결합**

둘 중 하나의 Observable에서 아이템이 배출될 때, 각 Observable에서 배출된 마지막 아이템을 특정 함수를 통해 결합하고, 그 결과 아이템을 배출한다.

---

결합되는 Observable이 적어도 하나의 아이템을 배출한 이후, 어떠한 소스 Observable이 아이템을 배출할 때마다 각 Observable에서 배출된 마지막 아이템을 특정 함수를 통해 결합하고 그 결과 아이템을 배출한다.

---

### Zip과의 차이

`Zip` 은 결합되는 Observable이 각각 이전에 결합되지 않은 아이템을 모두 배출하고 나서야 결합되지 않은 아이템을 결합한다.

---

각 Observable이 아이템을 최초로 배출한 이후, 어떠한 Observable이라도 아이템을 배출할 때 각 Observable이 마지막으로 배출한 아이템을 결합하고 싶을 때 사용한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<String>()
Observable.combineLatest(subject1, subject2) { "\($0)\($1)" }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

subject1.onNext(1)
subject1.onNext(2)
subject2.onNext("first")
subject1.onNext(3)
subject2.onNext("second")
subject2.onNext("third")

// next(2first)
// next(3first)
// next(3second)
// next(3third)
```

