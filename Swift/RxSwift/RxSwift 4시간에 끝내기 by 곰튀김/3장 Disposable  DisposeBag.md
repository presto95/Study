# 3장 Disposable / DisposeBag

- `Observable`의 `subscribe` 메소드는 `Disposable` 객체를 반환하며, 이것을 사용하여 `Observable` 시퀀스의 구독을 취소(unsubscribe)할 수 있다. 
- 이는 비동기적으로 수행되고 있던 작업을 취소하는 결과를 가져온다.
  - 이 작업은 `OperationQueue` 등을 사용하여도 구현 가능함

### DisposeBag

- `Disposable` 을 담는 가방
- 구독의 결과를 가방 안에 넣어두고, 이 가방을 초기화하는 방식으로 `Observable` 시퀀스를 초기화하는 것

```swift
let disposeBag = DisposeBag()
// DisposeBag 객체에 직접 Disposable 추가
// DisposeBag이 소멸될 때 dispose될 Disposable 추가
disposeBag.insert(disposable)
// subscribe 메소드의 반환값을 사용하여 바로 DisposeBag에 추가할 수 있음
......
	.observeOn(...)
	.subscribe(...)
	.dispose(by: disposeBag)	// Adds self to bag
```

