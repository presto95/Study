# From

**Observable 생성**

다양한 오브젝트들과 데이터 타입들을 Observable 내에 변환

---

iterator의 아이템들을 Observable에 내려줄 때 사용 가능하다.

아이템들을 Observable에 내려준 후 종료된다.

```swift
Observable<[Int]>.from([1, 2, 3])
	.subscribe { print($0) }

// next(1)
// next(2)
// next(3)
// completed
```

