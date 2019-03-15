# Buffer

### 분류

Observable 변형

### Observable 연산자 결정 트리

1. Observable이 배출하는 항목들을 모아둔 후 버퍼로 다시 배출해야 한다면

---

Observable이 배출하는 아이템들을 주기적으로 번들에 모아두고, 아이템을 배출하는 대신 이 번들을 한번에 배출

---

Observable 시퀀스의 각각의 아이템을 어떠한 버퍼에 투영하며, 버퍼가 가득 차거나 주어진 시간이 지났을 때 어떠한 버퍼의 생성이 종료된다.

`buffer(timeSpan:count:scheduler:)` 로 구현되어 있다. `timeSpan` 이 지나거나, `count` 만큼 버퍼가 가득 찼을 때 버퍼의 생성을 종료하고 생성된 버퍼를 내보낸다.

소스 Observable이 onError 알림을 발행한 경우, 그 이전에 소스 Observable이 배출한 아이템을 버퍼가 가지고 있을지라도, 그 아이템을 무시하고 에러를 배출한다.

---

### Window와의 차이

Window는 Buffer와 비슷하지만 아이템을 각각의 Observable로 모은다.

Buffer는 아이템을 밸출하기 이전 어떠한 자료 구조에 담는다.

---

주어진 시간 동안, 또는 원하는 개수만큼 Observable이 배출하는 아이템을 모아서 배출하고 싶을 때 사용한다.

```swift
let subject = PublishSubject<Int>()
subject.buffer(timeSpan: 0.3, count: 3, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
    
subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
subject.onNext(4)
subject.onError(NSError(domain: "", code: 0, userInfo: nil))

// next([1, 2, 3])
// error(Error Domain= Code=0 "(null)")
```

위의 경우 두 번째 버퍼 어셈블링 과정에서 `4` 가 정상적으로 들어갔음에도 이후에 에러가 발생하여 그 아이템을 무시하고 에러를 배출하는 것을 확인할 수 있다.