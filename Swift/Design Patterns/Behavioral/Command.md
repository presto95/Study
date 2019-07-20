# Command

### 명령 패턴

> 명령 패턴은 명령 객체에 요청 및 모든 필수 매개변수를 포함하여 요청을 표현하는 데 사용된다. 이 명령은 즉시 실행되거나 나중에 사용하기 위해 보류될 수 있다.

책임 연쇄 패턴과는 다르게 특정 명령을 수행하는 객체를 쌓아두고 행위자를 통해 호출한다.

수행 명령을 객체로 만들어 두고 처리 객체를 통해 실행한다.

명령에 필요한 인자 등은 모두 명령 객체에 포함된다.

행위가 위임되어 있기 때문에 객체 간의 의존성을 낮출 수 있다.

특히 원하는 연속 작업을 구성하기에 좋은 패턴이다.

### 코드

문의 열기 및 닫기 명령을 포함하는 객체를 각각 만들고 이를 실행할 처리 객체를 만든다.

각 명령은 DoorCommand 프로토콜로 추상화되었다.

코드를 이해하는데 큰 어려움은 없다.

```swift
protocol DoorCommand {
  func execute() -> String
}

final class OpenCommand: DoorCommand {
  let doors: String
  
  required init(doors: String) {
    self.doors = doors
  }
  
  func execute() -> String {
    return "Opened \(doors)"
  }
}

final class CloseCommand: DoorCommand {
  let doors: String
  
  required init(doors: String) {
    self.doors = doors
  }
  
  func execute() -> String {
    return "Closed \(doors)"
  }
}

final class HAL9000DoorsOperations {
  let openCommand: DoorCommand
  let closeCommand: DoorCommand
  
  init(doors: String) {
    openCommand = OpenCommand(doors: doors)
    closeCommand = CloseCommand(doors: doors)
  }
  
  func open() -> String {
    return openCommand.execute()
  }
  
  func close() -> String {
    return closeCommand.execute()
  }
}

/* 사용 */

let podBayDoors = "Pod Bay Doors"
let doorModule = HAL9000DoorsOperations(doors: podBayDoors)

doorModule.open()
doorModule.close()
```