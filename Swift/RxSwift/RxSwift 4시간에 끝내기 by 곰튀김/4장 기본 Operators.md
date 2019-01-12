# 4장 기본 Operators

## just

인자로 들어간 단일 요소를 포함하는 `Observable` 시퀀스 반환

```swift
Observable
	.just("Hello World", scheduler: MainScheduler.instance)
	// Observable<String>. "Hello World"가 스트림에 존재함
	.subscribe { event in
		switch event {
            case .next(let element):
            print(element)
            case .error(let error):
            print(error.localizedDescription)
            case .completed:
            break
        }
	}
	.disposed(by: disposeBag)
```

`subscribe` 메소드는 여러 형태를 가지고 있음

- `subscribe(on:)`
  -  `Event` 를 반환하여 이벤트의 각 경우(onNext, onError, onCompleted)에 대한 처리를 할 수 있음
- `subscribe(onNext:onError:onCompleted:onDisposed)`
  - 각 이벤트에 맞는 핸들러를 등록함. 모든 인자의 기본값은 `nil`.

## from

인자로 `Sequence` 를 요구하며 `Observable` 시퀀스를 반환

시퀀스의 요소가 하나하나 내려오게 됨: `stream`

```swift
Observable
	.from(["RxSwift", "In", 4, "Hours"])	// [Any]이나 상관 없음
	.subscribe(onNext: { element in
		print(element)
	})
	.disposed(by: disposeBag)
// RxSwift
// In
// 4
// Hours
```

## map

`Observable` 시퀀스의 각 요소를 새로운 형태로 만들어서 내림

## filter

`Observable` 시퀀스의 각 요소를 필터링하여 내림