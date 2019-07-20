# Protection Proxy

### 보호 프록시 패턴

> 프록시 패턴은 대리 또는 플레이스홀더 객체를 제공하기 위해 사용되는데, 이는 기반 객체를 참조한다. 보호 프록시는 접근을 제한하는 것이다.

### 프록시

- 다른 무언가와 이어지는 인터페이스 역할을 하는 클래스
- 어떠한 것과도 인터페이스의 역할을 수행할 수 있음

권한에 따른 접근을 통해 보호를 제공한다.

프록시와 실제 수행 객체는 같은 Base를 갖는다.

### 코드

```swift
// Base.
protocol DoorOpening {
  func open(doors: String) -> String
}

// 실제 수행 객체.
final class HAL9000: DoorOpening {
  func open(doors: String) -> String {
    return ("HAL9000: Affirmative, Dave. I read you. Opened \(doors).")
  }
}

// 프록시.
final class CurrentComputer: DoorOpening {
  private var computer: HAL9000!
  
  func authenticate(password: String) -> Bool {
    guard password == "pass" else {
      return false
    }
    // 권한이 확인된 후 인스턴스를 할당하여 접근을 가능하게 함.
    computer = HAL9000()
    return true
  }
  
  func open(doors: String) -> String {
    guard computer != nil else {
      return "Access Denied. I'm afraid I can't do that."
    }
    return computer.open(doors: doors)
  }
}

/* 사용 */

let computer = CurrentComputer()
let podBay = "Pod Bay Doors"

// 권한이 확인되지 않았으므로 접근이 허용되지 않음.
computer.open(doors: podBay)

computer.authenticate(password: "pass")
computer.open(doors: podBay)
```