# Error

### 분류

Observable 생성

Throw의 변형

### Observable 연산자 결정 트리

1. 오류가 발생했을 때 Observable이 옵저버에게 오류를 알려야 한다면

---

아이템을 배출하지 않고 에러와 함께 종료되는 Observable 생성

---

에러를 발생시키며 종료되는 Observable을 생성한다.

```swift
Observable<Int>.error(NSError(domain: "", code: 0, userInfo: nil))
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// error(Error Domain= Code=0 "(null)")
```

