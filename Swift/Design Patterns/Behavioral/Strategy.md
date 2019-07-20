# Strategy

### 전략 패턴

> 전략 패턴은 필요한 프로세스가 런타임에서 선택되는 상호 교환 가능한 알고리즘 패밀리를 생성하기 위해 사용된다.

런타임에서 알고리즘을 선택할 수 있게 한다.

특정 계열의 알고리즘을 정의하고 (is-a 관계) / 각 알고리즘을 캡슐화하고 / 해당 계열 안에서 상호 교체 가능하게 한다.

전략은 알고리즘을 사용하는 클라이언트에 독립적이다.

클라이언트가 직접 전략 객체를 지정하여 변환을 시도한다.

### 상태 패턴과의 차이

상태 패턴은 상태 객체에서 수행 후 컨텍스트 객체에 상태를 가져온다.

전략 패턴은 컨텍스트에 전략 객체를 넣고 컨텍스트를 통해 수행한다.

### 코드

전략을 추상화하는 RealnessTesting 프로토콜을 정의하고, 이를 채택하는 각각의 구체 전략을 구현한다.

수행하는 객체는 이 전략을 주입받아 특정 전략에 의해 로직을 수행할 수 있도록 한다.

이해하는 데 큰 어려움이 없다.

```swift
struct TestSubject {
  let pupilDiameter: Double
  let blushResponse: Double
  let isOrganic: Bool
}

protocol RealnessTesting: class {
  func testRealness(_ testSubject: TestSubject) -> Bool
}

final class VoightKampffTest: RealnessTesting {
  func testRealness(_ testSubject: TestSubject) -> Bool {
    return testSubject.pupilDiameter < 30 || testSubject.blushResponse == 0
  }
}

final class GeneticTest: RealnessTesting {
  func testRealness(_ testSubject: TestSubject) -> Bool {
    return testSubject.isOrganic
  }
}

// 컨텍스트 클래스.
final class BladeRunner {
  private let strategy: RealnessTesting
  
  init(test: RealnessTesting) {
    strategy = test
  }
  
  func testIfAndroid(_ testSubject: TestSubject) -> Bool {
    return !strategy.testRealness(testSubject)
  }
}

/* 사용 */

let rachel = TestSubject(pupilDiameter: 30.2, blushResponse: 0.3, isOrganic: false)

let deckard = BladeRunner(test: VoightKampffTest())
let isRachelAndroid = deckard.testIfAndroid(rachel)

let gaff = BladeRunner(test: GeneticTest())
let isDeckardAndroid = gaff.testIfAndroid(rachel)
```