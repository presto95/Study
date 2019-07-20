# Virtual Proxy

### 가상 프록시 패턴

> 프록시 패턴은 대리 또는 플레이스홀더 객체를 제공하기 위해 사용되는데, 이는 기반 객체를 참조한다. 가상 프록시는 요청에 따라 객체를 불러오기 위해 사용된다.

### 코드

지연 저장 프로퍼티를 활용하여 메소드가 호출될 때 객체를 생성하도록 한다.

```swift
protocol HEVSuitMedicalAid {
  func administerMorphine() -> String
}

final class HEVSuit: HEVSuitMedicalAid {
  func administerMorphine() -> String {
    return "Morphine administered."
  }
}

final class HEVSuitHumanInterface: HEVSuitMedicalAid {
  lazy private var physicalSuit: HEVSuit = HEVSuit()
  
  func administerMorphine() -> String {
    return physicalSuit.administerMorphine()
  }
}

/* 사용 */

let humanInterface = HEVSuitHumanInterface()
humanInterface.administerMorphine()
```

