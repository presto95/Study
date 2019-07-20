# Observer

### 옵저버 패턴

> 옵저버 패턴은 객체가 그 상태 변화를 낼 수 있도록 하기 위해 사용된다. 다른 객체는 어떠한 변화에 즉시 알림 받기 위해 구독한다.

일대다 객체 관계에서 하나의 객체가 수정되었을 때 이에 의존하는 객체들이 자동으로 통보를 받는 구조를 갖는다.

발행 / 구독 모델 (Publish / Subscribe)

분산 이벤트 핸들링 시스템 구현

iOS에서도 많은 곳에서 이 패턴을 찾아볼 수 있다. (NotificationCenter 등)

### 코드

Swift의 프로퍼티 감시자를 활용하면 상태의 변경 직전과 직후를 관찰할 수 있다.

콜백을 등록하는 대신, 관찰을 위한 프로토콜을 정의하고 이를 구현하는 클래스를 옵저버로 사용한다. (델리게이트 패턴)

이해하는 데 큰 어려움이 없다.

```swift
protocol PropertyObserver: class {
  func willChange(propertyName: String, newPropertyValue: Any?)
  func didChange(propertyName: String, oldPropertyValue: Any?)
}

final class TestChambers {
  weak var observer: PropertyObserver?
  
  private let testChamberNumberName = "testChamberNumber"
  
  var testChamerNumber: Int = 0 {
    willSet {
      observer?.willChange(propertyName: testChamberNumberName, newPropertyValue: newValue)
    }
    didSet {
      observer?.didChange(propertyName: testChamberNumberName, oldPropertyValue: oldValue)
    }
  }
}

final class Observer: PropertyObserver {
  func willChange(propertyName: String, newPropertyValue: Any?) {
    if newPropertyValue as? Int == 1 {
      print("Okay. Look. We both said a lot of things that you're going to regret.")
    }
  }
  
  func didChange(propertyName: String, oldPropertyValue: Any?) {
    if oldPropertyValue as? Int == 0 {
      print("Sorry about the mess. I've really let the place go since you killed me.")
    }
  }
}

/* 사용 */

var observerInstance = Observer()
var testChambers = TestChambers()
testChambers.observer = observerInstance
testChambers.testChamberNumber += 1
```