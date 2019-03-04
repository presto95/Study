# FlatMap

**Observable 변환**

어떠한 Observable이 배출한 아이템들을 Observable로 변환한다. 그리고 나서 그것들을 하나의 Observable로 평평하게 한다.

---

소스 Observable이 배출한 각 아이템에 특정 함수를 적용한 결과를 Observable로 변환하며, 변환된 Observable들은 평평하게 되어 단일 Observable을 만들어낸다.

소스 Observable의 아이템이 특정 함수를 적용한 아이템이 흐르는 Observable로 변환되는 것이다. 변환된 각 Observable은 종료되는 것이다.

FlatMap은 이러한 Observable들이 배출한 것을 merge하므로, 각 아이템이 변환된 순서를 보장하지 않는다.

매우 유용하게 사용된다.

---

### Map과의 차이

둘 모두 Observable을 변환하는 오퍼레이터이지만,

Map은 Observable이 배출한 아이템에 특정 함수를 적용한 결과로 아이템을 변형하는 것이며,

FlatMap은 Observable이 배출한 아이템에 특정 함수를 적용한 결과 아이템이 흐르는 Observable로 아이템을 변형하며, 변환된 Observable들을 평평하게 만드는 것이다.

Map에 전달되는 클로저의 반환 타입은 `ObservableConvertibleType` 을 준수하지 않고, FlatMap에 전달되는 클로저의 반환 타입은 `ObservableConvertibleType`을 준수한다.

---

Map처럼 자주 사용되나 잘 사용하는 것이 매우 중요할 것 같다.

```swift
Observable.from([1, 2, 3])
  .flatMap { Observable.just(Double($0)) }
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1.0)
// next(2.0)
// next(3.0)
// completed
```

