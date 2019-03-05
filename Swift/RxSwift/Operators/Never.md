# Never

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 아무 일도 하지 않아야 한다면

---

어떠한 아이템도 배출하지 않으며 종료하지 않는 Observable 생성

---

종료하지 않는 스트림을 만든다.

```swift
Observable<Int>.never()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// 출력 없음
```

