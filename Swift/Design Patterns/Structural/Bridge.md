# Bridge

### 브릿지 패턴

> 브릿지 패턴은 클래스의 추상화된 요소를 세부 구현과 분리시키기 위해 사용되며, 이는 추상화를 수정하지 않고 세부 구현을 교체할 수 있다는 의미를 제공한다.

구현부에서 추상층을 분리하여 각자 독립적으로 변형할 수 있게 한다.

### 코드

```swift
protocol Switch {
  var appliance: Appliance { get set }
  func turnOn()
}

protocol Appliance {
  func run()
}

final class RemoteControl: Switch {
  var appliance: Appliance
  
  init(appliance: Appliance) {
    self.appliance = appliance
  }
  
  func turnOn() {
    appliance.run()
  }
}

final class TV: Appliance {
  func run() {
    print("tv turned on")
  }
}

final class VacuumCleaner: Appliance {
  func run() {
    print("vacuum cleaner turned on")
  }
}

/* 사용 */

let tvRemoteControl = RemoteControl(appliance: TV())
tvRemoteControl.turnOn()

let fancyVacuumCleanerRemoteControl = RemoteControl(appliance: VacuumCleaner())
fancyVacuumCleanerRemoteControl.turnOn()
```