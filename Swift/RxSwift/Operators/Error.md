# Error

**Observable 생성**

아이템을 배출하지 않고 에러와 함께 종료되는 Observable 생성

---

에러를 발생시키며 종료되는 Observable을 생성한다.

```swift
Observable<Int>.error(NSError(domain: "", code: 0, userInfo: nil))
	.subscribe { print($0) }
// error(Error Domain= Code=0 "(null)")
```

