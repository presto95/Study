# Just

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 특정 항목을 생성해야 한다면

---

특정 아이템을 배출하는 Observable 생성

---

어떠한 아이템을 그 아이템을 배출하는 Observable로 변환하는 오퍼레이터.

생성된 Observable은 해당 아이템을 배출하고 종료된다.

---

### From과의 차이

- 아이템 그 자체를 배출함. 그것이 어레이인지, 단일 항목인지에 상관 없이 해당 아이템 자체를 배출하는 Observable로 변환한다.

---

어떠한 요소만을 배출하고 종료되는 스트림을 만들고 싶을 때 사용한다.

```swift
Observable<Int>.just(1)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// completed

Observable<[Int]>.just([1, 2])
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next([1, 2])
// completed
```

