# Memento

### 메멘토 패턴

> 메멘토 패턴은 캡슐화 규칙을 어기지 않고 나중에 다시 획득할 수 있는 어떠한 방법으로 객체의 현재 상태를 획득하고 저장하기 위해 사용된다.

객체를 이전 상태로 되돌릴 수 있는 기능을 제공한다. (롤백을 통한 실행 취소)

Originator / Caretaker / Memento라고 불리는 세 개의 객체로 구현된다.

- Originator
  - 내부 상태 객체
- Caretaker
  - 원하는 곳으로 돌아갈 정보를 담고 있는 객체
  - Originator에 대해 무언가를 하지만 변경에 대한 실행 취소를 하기를 원함
  - 먼저 Originator에게 Memento 객체를 요청하고 예정된 일련의 명령을 수행함
  - 명령 이전의 상태로 되돌리기 위해 Memento 객체를 Originator에 반환함
- Memento
  - 현재 정보를 담고 있는 객체
  - 불투명 자료형

상태 값을 저장하고 복원하기만을 위한 패턴이다.

### 코드

- 현재 정보인 Memento는 String 키와 String 값을 갖는 딕셔너리 타입이다.
- 내부 상태 객체인 Originator는 상태를 갖도록 한다.
- Caretaker에 Originator들을 저장하여 원하는 곳으로 돌아갈 정보를 담을 수 있도록 한다. 데이터 저장을 위해 UserDefaults를 활용한다.

#### Memento

```swift
typealias Memento = [String: String]
```

#### Originator

```swift
protocol MementoConvertible {
  var memento: Memento { get }
  init?(memento: Memento)
}

struct GameState: MementoConvertible {
  private enum Keys {
    static let chapter = "com.value.halflife.chapter"
    static let weapon = "com.value.halflife.weapon"
  }
  
  var chapter: String
  var weapon: String
  
  init(chapter: String, weapon: String) {
    self.chapter = chapter
    self.weapon = weapon
  }
  
  init?(memento: Memento) {
    guard let mementoChapter = memento[Keys.chapter], let mementoWeapon = memento[Keys.weapon] else {
      return nil
    }
    chapter = mementoChapter
    weapon = mementoWeapon
  }
  
  var memento: Memento {
    return [Keys.chapter: chapter, Keys.weapon: weapon]
  }
}
```

#### Caretaker

```swift
enum CheckPoint {
  private static let defaults = UserDefaults.standard
  
  static func save(_ state: MementoConvertible, saveName: String) {
    defaults.set(state.memento, forKey: saveName)
    defaults.synchronize()
  }
  
  static func restore(saveName: String) -> Any? {
    return defaults.object(forKey: saveName)
  }
}
```

#### Usage

```swift
// 내부 상태 객체 `Originator` 객체를 만듦.
var gameState = GameState(chapter: "Black Mesa Inbound", weapon: "Crowbar")

gameState.chapter = "Anomalous Materials"
gameState.weapon = "Glock 17"
// 원하는 곳으로 돌아갈 정보를 담기 위한 `Caretaker` 객체를 활용하여 상태 저장.
CheckPoint.save(gameState, saveName: "gameState1")

gameState.chapter = "Unforeseen Consequences"
gameState.weapon = "MP5"
// 원하는 곳으로 돌아갈 정보를 담기 위한 `Caretaker` 객체를 활용하여 상태 저장.
CheckPoint.save(gameState, saveName: "gameState2")

gameState.chapter = "Office Complex"
gameState.weapon = "Crossbow"
// 원하는 곳으로 돌아갈 정보를 담기 위한 `Caretaker` 객체를 활용하여 상태 저장.
CheckPoint.save(gameState, saveName: "gameState3")

// 원하는 곳 `gameState`으로 돌아가기 위한 로직.
if let memento = CheckPoint.restore(saveName: "gameState1") as? Memento {
  let finalState = GameState(memento: memento)
  dump(finalState)
}
```