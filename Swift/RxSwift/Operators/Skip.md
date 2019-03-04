# Skip

**Observable 필터링**

Observable이 배출한 최초의 n개의 아이템을 버리기

---

RxSwift에서 Skip은 두 가지 형태로 존재한다.

1. Observable이 배출한 최초의 n개의 아이템을 무시하기
2. Observable의 시작에서 주어진 시간 동안 배출되는 아이템을 무시하기

---

Observable이 배출한 최초의 n개의 아이템을 무시하고 싶을 때, 특정 시간 동안 배출된 아이템을 무시하고 싶을 때 사용한다.

```swift
// 1
Observable.from[1, 2, 3, 4]
  .skip(2)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(3)
// next(4)
// completed
```

```swift
// 2
let subject = PublishSubject<Int>()
  subject
    .skip(0.5, scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
subject.onNext(1)
Timer.scheduledTimer(withTimeInterval: 1, repeats: false) { _ in
  subject.onNext(2)
}

// next(2)
```

