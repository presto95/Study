# ConcatMap

### 분류

수학 및 집계 연산자

Concat의 변형

### Observable 연산자 결정 트리

1. Observable이 배출한 항목들을 변환한 후에 다시 배출해야 하는데
2. 순서대로 Observable이 배출한 항목들을 연결지어 배출해야 한다면

---

flatMap과 비슷한 동작을 하나, concatMap은 리시버 Observable이 배출하는 아이템에 적용한 함수를 통해 만들어진 Observable이 배출하는 아이템의 순서를 보장한다.

---

### FlatMap과의 차이

FlatMap : 어떠한 Observable 시퀀스의 각 아이템을 어떠한 Observable로 투영한 후, 결과 Observable 시퀀스들을 하나의 Observable 시퀀스로 **병합merge한다.**

ConcatMap : 어떠한 Observable 시퀀스의 각 아이템을 어떠한 Observable로 투영한 후, 결과 Observable 시퀀스들을 하나의 Observable 시퀀스로 **연결concatenate한다.**

merge와 concatenate의 차이만 존재한다. FlatMap은 변환 아이템의 순서를 보장하지 않으며, ConcatMap은 변환 아이템의 순서를 보장한다.

---

FlatMap의 동작을 원하나, 그 순서를 보장하고 싶을 때 사용한다.

```swift
Observable.just(1)
  .concatMap { value -> Observable<Double> in
    return Observable.from([Double](repeating: Double(value), count: 3))
  }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1.0)
// next(1.0)
// next(1.0)
// completed
```

