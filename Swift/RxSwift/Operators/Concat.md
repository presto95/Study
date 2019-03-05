# Concat

### 분류

수학 및 집계 연산자

### Observable 연산자 결정 트리

1. 다른 Observable을 결합시켜 새로운 Observable을 생성해야 한다
2. 생성하고 싶은 Observable은 두 개 이상의 Observable이 가진 항목들을 순서대로 결합시켜 새로운 항목을 배출하는 것일 때

---

두 개 이상의 Observable이 배출하는 아이템들을 이어붙여 배출

---

Concat에 주어진 인자에 따라 아이템 배출 순서가 보장된다.

이전 Observable이 완료되는 시점에서 다음 Observable의 아이템이 배출된다.

RxSwift에는 두 가지 형태로 구현되어 있다.

1. `Observable.concat` 의 형태로 여러 Observable을 이어붙이기
2. 리시버 Observable에 concat 오퍼레이터를 사용하여 주어진 Observable을 이어붙이기

---

이전 Observable이 완료되지 않는 Observable이면 정상적으로 동작하지 않을 것이다.

---

여러 개의 Observable을 이어붙이고 싶을 때 사용한다.

```swift
Observable.concat([
  Observable.just(1),
  Observable.from([2, 3, 4])
  ])
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// next(4)
// completed
```

```swift
Observable.from([1, 2, 3])
  .concat(Observable.just(4))
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// next(4)
// completed
```

