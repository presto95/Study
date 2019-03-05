# IfEmpty

### 분류

조건 및 불리언 연산자

DefaultIfEmpty의 변형

---

RxSwift에서는 두 가지의 형태로 존재한다.

1. 소스 Observable이 어떠한 아이템도 배출하지 않는다면, 주어진 기본값을 배출함
2. 소스 Observable이 어떠한 아이템도 배출하지 않는다면, 주어진 Observable로 전환함

---

소스 Observable이 비어 있을 때의 처리를 편리하게 할 수 있다.

```swift
Observable<Int>.empty()
  .ifEmpty(1)
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// completed
```

```swift
Observable<Int>.empty()
  .ifEmpty(switchTo: Observable<Int>.from([1, 2, 3]))
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// completed
```

```swift
Observable<Int>.just(1)
  .ifEmpty(switchTo: Observable<Int>.from([1, 2, 3]))
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// completed
```

