# 단위 테스트

#### Unit Tests

## 커스텀 오퍼레이터 테스트

#### Testing custom operators

RxSwift는 모든 오퍼레이터 테스트를 위해 `RxTest` 를 사용합니다. 이는 `Rx.workspace` 프로젝트의 AllTests-* 타겟에 위치해 있습니다.

전형적인 `RxSwift` 오퍼레이터의 단위 테스트 예제입니다.

```swift
func testMap_Range() {
  // 테스트 스케줄러 초기화.
  // 테스트 스케줄러는 로컬 머신 시계로부터 분리된 가상 시간을 구현한다.
  // 이는 시뮬레이션을 가능한 한 빠르게 실행하고 모든 이벤트가 처리되었음을 입증할 수 있게 한다. 
  let scheduler = TestScheduler(initialClock: 0)
    
  // 가짜 hot observable 시퀀스 생성.
  // 시퀀스는 구독하는 옵저버가 있든 없든, 지정된 시간에 이벤트를 배출할 것이다.
  // (이것이 hot이 의미하는 것)
  // 이 옵저버블 시퀀스는 또한 그 라이프타임 동안 발생한 모든 구독을 기록할 것이다.
  // (`subscriptions` 프로퍼티)
  let xs = scheduler.createHotObservable([
    .next(150, 1),  // 첫 번째 인자는 가상 시간, 두 번째 인자는 요소 값
    .next(210, 0),
    .next(220, 1),
    .next(230, 2),
    .next(240, 4),
    .completed(300) // 완료 이벤트가 발생할 때의 가상 시간
  ])
    
  // `start` 메소드는 기본적으로 다음과 같이 동작한다.
  // * 시뮬레이션을 돌리고 `res`가 참조하는 옵저버를 사용하여 모든 이벤트를 기록함
  // * 200의 가상 시간에 구독
  // * 1000의 가상 시간에 구독 해제
  let res = scheduler.start { xs.map { $0 * 2 } }
  
  let correctMessages = Recorded.events(
    .next(210, 0 * 2),
    .next(220, 1 * 2),
    .next(230, 2 * 2),
    .next(240, 4 * 2),
    .completed(300)
  )
    
  let correctSubscriptions = [
    Subscription(200, 300)
  ]
  
  XCTAssertEqual(res.events, correctMessages)
  XCTAssertEqual(xs.subscriptions, correctSubscriptions)
}
```

이벤트 시간에 관심이 없는 종료되지 않는 시퀀스의 경우, 배출되는 특정 요소를 assert하기 위해 `RxTest` 의 `XCTAssertRecordedElements` 를 사용할 수 있을 것이다. 종료하는 중단 이벤트 (예, `completed` 또는 `error`) 는 테스트를 실패하도록 할 것이다.

```swift
func testElementsEmitted() {
  let scheduler = TestScheduler(initialClock: 0)

  let xs = scheduler.createHotObservable([
    .next(210, "RxSwift"),
    .next(220, "is"),
    .next(230, "pretty"),
    .next(240, "awesome")
  ])

  let res = scheduler.start { xs.asObservable() }

  XCTAssertRecordedElements(res.events, ["RxSwift", "is", "pretty", "awesome"])
}
```

## 오퍼레이터 조합 테스트 (뷰 모델, 컴포넌트)

#### Testing operator compositions (view models, components)

오퍼레이터 조합을 테스트하는 방법에 대한 예제는 `Rx.xcworkspace` > `RxExample-iOSTests` 타겟이 포함하고 있습니다.

`RxTest` 의 익스텐션을 정의하여 가독성 좋은 방법으로 테스트틀 쉽게 작성할 수 있게 할 수 있습니다. `RxExample-iOSTests` 가 제공하는 예제는 그러한 익스텐션을 작성하는 방법에 대한 제안일 뿐이며, 그러한 테스트를 작성하는 방법은 다양합니다.

```swift
// 기대되는 이벤트 및 테스트 데이터
let (
  usernameEvents,
  passwordEvents,
  repeatedPasswordEvents,
  loginTapEvents,

  expectedValidatedUsernameEvents,
  expectedSignupEnabledEvents
) = (
  scheduler.parseEventsAndTimes("e---u1----u2-----u3-----------------", values: stringValues).first!,
  scheduler.parseEventsAndTimes("e----------------------p1-----------", values: stringValues).first!,
  scheduler.parseEventsAndTimes("e---------------------------p2---p1-", values: stringValues).first!,
  scheduler.parseEventsAndTimes("------------------------------------", values: events).first!,

  scheduler.parseEventsAndTimes("e---v--f--v--f---v--o----------------", values: validations).first!,
  scheduler.parseEventsAndTimes("f--------------------------------t---", values: booleans).first!
)
```

## 통합 테스트

#### Integration tests

`RxBlocking` 오퍼레이터를 사용하여 통합 테스트를 작성할 수 있습니다.

`RxBlocking` 의 `toBlocking()` 메소드를 사용하여 현재 스레드를 가로막고 시퀀스가 완료되는 것을 기다릴 수 있으며, 이는 동기적으로 그 결과에 접근할 수 있게 해줍니다.

시퀀스의 결과를 테스트하는 간단한 방법은 `toArray` 메소드를 사용하는 것입니다. 이는 시퀀스가 성공적으로 완료될 때 배출되는 모든 요소의 배열을 반환할 것이며, 또는 시퀀스가 종료될 때 발생하는 에러를 `throw` 할 것입니다.

```swift
let result = try fetchResource(location)
  .toBlocking()
  .toArray()

XCTAssertEqual(result, expectedResult)
```

또다른 선택지는 `materialize` 오퍼레이터를 사용하여 시퀀스를 더욱 잘게 쪼개어 시험할 수 있게 하는 것입니다. 이는 `MaterializedSequenceResult` 열거형을 반환하며, 시퀀스가 성공적으로 완료될 때 대출되는 요소와 함께 하는 `.completed` 또는 시퀀스가 에러와 함께 종료되었다면 배출된 에러와 함께 하는 `.failed` 가 될 수 있습니다.

```swift
let result = try fetchResource(location)
  .toBlocking()
  .materialize()

// 에러와 함께 종료된 경우 결과 또는 에러를 테스트하기 위함
switch result {
case .completed:
  XCTFail("Expected result to complete with error, but result was successful.")
case .failed(let elements, let error):
  XCTAssertEqual(elements, expectedResult)
            XCTAssertErrorEqual(error, expectedError)
}

// 완료와 함께 종료된 경우 결과를 테스트하기 위함
switch result {
case .completed(let elements):
  XCTAssertEqual(elements, expectedResult)
case .failed(_, let error):
  XCTFail("Expected result to complete without error, but received \(error).")
}
```