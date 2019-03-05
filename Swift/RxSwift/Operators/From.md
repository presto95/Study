# From

### 분류

Observable 생성

### Observable 연산자 결정 트리

1. 새로운 Observable을 생성하고 싶은데 그 Observable이
2. 특정 Array, Iterable 또는 유사한 형태의 소스로부터 항목들을 배출해야 한다면

---

다양한 오브젝트들과 데이터 타입들을 Observable 내에 변환

---

iterator의 아이템들을 Observable에 내려줄 때 사용 가능하다.

아이템들을 Observable에 내려준 후 종료된다.

```swift
Observable<[Int]>.from([1, 2, 3])
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// next(1)
// next(2)
// next(3)
// completed
```

