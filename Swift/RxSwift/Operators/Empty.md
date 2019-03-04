# Empty

**Observable 생성**

아이템을 배출하지 않고 정상적으로 종료되는 Observable 생성

---

정상적으로 종료되는 Observable을 생성한다.

```swift
Observable<Int>.empty()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// completed

Observable<String>.empty()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// completed
```

