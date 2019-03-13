# StartWith

### 분류

Observable 결합

### Observable 연산자 결정 트리

1. Observable이 가진 항목 그대로를 배출하지만 배출 전에 다른 항목들을 먼저 배출할 수 있도록 추가해야 한다면

---

소스 Observable의 아이템 배출 시작 전에 특정 아이템 시퀀스를 배출

---

어떠한 Observable이 아이템을 배출하기 전에 특정 아이템 시퀀스를 배출하기 원할 때 사용한다.

---

아이템 시퀀스에 특정 아이템 시퀀스를 이어붙이는 것을 원한다면 Concat 오퍼레이터를 참고해보자.

```swift
Observable<Int>.just(1)
  .startWith(2, 3)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(2)
// next(3)
// next(1)
// completed
```

