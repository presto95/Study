# 7장 Next / Error / Completed

`subscribe`

- 스트림을 구독
- `Disposable` 을 반환

```swift
// 스트림을 구독하나 별다른 처리를 하지 않음
subscribe()
// 스트림을 구독하여 이벤트에 따라 어떠한 작업을 수행
// .next, .error, .completed
subscribe(on:)
// 각 이벤트에 대한 핸들러 등록
subscribe(onNext:onError:onCompleted:onDisposed)
subscribe(observer:)
```

**스트림은 에러가 발생하거나 완료되면 dispose된다.**

**`DisposeBag` 에 들어간 `Disposable` 을 초기화하면 dispose된다.**

