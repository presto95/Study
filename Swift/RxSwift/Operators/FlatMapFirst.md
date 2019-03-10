# FlatMapFirst

### 분류

Observable 변환

FlatMap의 변형

---

FlatMap과 비슷한 동작을 취하나, 소스 Observable이 배출한 어떠한 아이템에 대하여 FlatMap 동작을 취하는 도중 다른 아이템을 배출한다면, 이 아이템은 무시된다.

---

### FlatMap과의 차이

FlatMap은 소스 Observable이 아이템을 배출하는 시간과 순서에 따라 어떠한 동작을 취하며, 변환된 Observable의 아이템 순서를 보장하지 않는다.

FlatMapFirst는 변환 작업이 끝나기 전에 소스 Observable이 아이템을 배출하면, 그 아이템에 대한 변환은 무시된다.

---

[민소네님 블로그](http://minsone.github.io/programming/reactive-swift-flatmap-flatmapfirst-flatmaplatest)에 정리가 정말 잘 되어 있다!

```swift
let o1 = Observable<Int>
  .interval(0.5, scheduler: MainScheduler.instance)
  .take(4)
o1.flatMapFirst { element -> Observable<Int> in
  let o2 = Observable<Int>
    .interval(0.2, scheduler: MainScheduler.instance)
    .take(4)
    .map { _ in element }
  return o2
}
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(0)
// next(0)
// next(0)
// next(0)
// next(2)
// next(2)
// next(2)
// next(2)
// completed
```

`o1` Observable은 0.5초의 간격으로 0부터 시작하는 정수를 무한히 배출하며, `take(4)` 오퍼레이터를 통해 4개의 값만을 취하고 종료되게 된다.

그 `o1` Observable에 FlatMapFirst를 적용하여, 각 아이템이 배출될 때 0.2초 간격으로 0부터 시작하는 정수를 무한히 배출하며, 4개의 값만을 취하고, 그 값들을 `o1` Observable이 배출한 정수 값으로 변환하여 배출하며, 종료되는 Observable로 변환하도록 하였다.

이 경우 `o1` Observable이 최초에 0을 배출하여 FlatMapFirst에 따라 변환되는 도중 1을 배출할 때, 0에 대한 변환 작업이 완료되지 않은 상태이므로 1은 무시된다. 이후 2를 배출했을 때는 0에 대한 변환 작업이 완료되었으므로 2에 대한 변환 작업이 실행되며, 3을 배출했을 때는 2에 대한 변환 작업이 완료되지 않았으므로 무시된다.