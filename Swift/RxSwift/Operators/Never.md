# Never

**Observable 생성**

어떠한 아이템도 배출하지 않으며 종료하지 않는 Observable 생성

---

종료하지 않는 스트림을 만든다.

```swift
Observable<Int>.never()
  .subscribe { print($0) }
  .disposed(by: disposeBag)

// 출력 없음
```

