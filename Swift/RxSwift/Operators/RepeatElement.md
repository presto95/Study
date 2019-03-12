# RepeatElement

### 분류

Observable 생성

Repeat의 변형

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 반복적으로 연속된 항목을 배출해야 한다면

---

특정 아이템을 반복적으로 배출한다. 무한 루프를 도는 느낌.

스케줄러를 지정해줄 수 있다. 기본값은  `CurrentThreadScheduler.instance` 이다.

종료되지 않는 Observable을 만들어 낸다.

---

어떠한 아이템을 반복적으로 배출하게 할 때 사용한다.

```swift
Observable<Int>.repeatElement(1)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(1)
// next(1)
// next(1)
// ...
// 위의 결과가 반복된다.
```

```swift
Observable<Int>.repeatElement(1)
  .take(3)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(1)
// next(1)
// 
```

