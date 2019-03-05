# Empty

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 항목 배출 없이 실행을 완료해야 한다면

---

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

