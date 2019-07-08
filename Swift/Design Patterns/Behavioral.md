# Behavioral

#### 행위 / 동작 패턴

> 소프트웨어 공학에서, 동작 패턴은 오브젝트 간 일반적인 상호 작용 패턴을 식별하고 그러한 것을 구현하는 디자인 패턴이다. 이렇게 하는 것으로 상호 작용을 하는 데 있어서 유연성을 증가시킨다.

클래스의 행위와 관련된 패턴. 시스템 및 프레임워크 개발 및 이해에 많은 도움을 줄 수 있을 것이다.

## Chain of Responsibility

#### 책임 연쇄 패턴

> 책임 연쇄 패턴은 다양한 요청을 처리하기 위해 사용되는데, 각 요청은 다른 핸들러에 의해 처리되도록 한다.

명령 객체들과 처리 객체들로 이루어져 있다.

각 처리 객체들은 명령 객체들의 타입을 정의하며, 처리가 되지 않은 것들을 다음 처리 객체에 전달한다.

느슨한 결합을 구현하기 위해 사용되는 좋은 패턴이다.

### 코드

ATM에서 입력한 금액을 인출하는데, 단위가 큰 것부터 돈을 계산하여 준다. (100 -> 50 -> 20 -> 10)

100달러 처리 객체는 50달러 처리 객체를, 50달러 처리 객체는 20달러 처리 객체를, 20달러 처리 객체는 10달러 처리 객체를 갖도록 하여 해당 객체에서 처리하지 못하는 경우 다음 처리 객체에 전달할 수 있도록 한다.

```swift
protocol Withdrawing {
  // `amount` 금액 인출.
  // 금액을 인출할 수 없으면 false를 반환한다.
  func withdraw(amount: Int) -> Bool
}

final class MoneyPile: Withdrawing {
  // 화폐 가치. 100 / 50 / 20 / 10.
  let value: Int
  // 화폐 개수.
  var quantity: Int
  // 다음 처리 객체.
  var next: Withdrawing?
  
  init(value: Int, quantity: Int, next: Withdrawing?) {
    self.value = value
    self.quantity = quantity
    self.next = next
  }
  
  func withdraw(amount: Int) -> Bool {
    var amount = amount
    var quantity = self.quantity
    // 입력으로 들어온 금액을 현재 객체의 화폐 가치로 처리할 수 있는 동안,
    while amount / value > 0 {
      // 남은 화폐가 없는 경우 break.
      if quantity == 0 {
        break
      }
      // 처리.
      amount -= value
      quantity -= 1
    }
    // 처리 종료. 남은 금액에 대한 처리.
    // 남은 금액이 없으면 (현재 객체에서 모든 처리를 완료했으면) true 반환하여 처리 종료.
    guard amount > 0 else {
      return true
    }
    // 남은 금액이 있고 다음 처리 객체가 있으면 다음 처리 객체에 남은 금액에 대한 처리를 넘겨줌.
    if let next = next {
      return next.withdraw(amount: amount)
    }
    // 모든 경우에 맞지 않는 경우 false 반환하여 처리할 수 없음을 나타냄.
    return false
  }
}

final class ATM: Withdrawing {
  private var hundred: Withdrawing
  private var fifty: Withdrawing
  private var twenty: Withdrawing
  private var ten: Withdrawing
  
  private var startPile: Withdrawing {
    return hundred
  }
  
  init(hundred: Withdrawing, fifty: Withdrawing, twenty: Withdrawing, ten: Withdrawing) {
    self.hundred = hundred
    self.fifty = fifty
    self.twenty = twenty
    self.ten = ten
  }
  
  func withdraw(amount: Int) -> Bool {
    // 100 단위부터 처리 로직 실행.
    return startPile.withdraw(amount: amount)
  }
}

/* 사용 */

// 10의 가치를 갖는 화폐를 6개 생성. 다음 처리 객체는 없음.
let ten = MoneyPile(value: 10, quantity: 6, next: nil)
// 20의 가치를 갖는 화폐를 2개 생성. 다음 처리 객체는 ten.
let twenty = MoneyPile(value: 20, quantity: 2, next: ten)
// 50의 가치를 갖는 화폐를 2개 생성. 다음 처리 객체는 twenty.
let fifty = MoneyPile(value: 50, quantity: 2, next: twenty)
// 100의 가치를 갖는 화폐를 1개 생성. 다음 처리 객체는 fifty.
let hundred = MoneyPile(value: 100, quantity: 1, next: fifty)

let atm = ATM(hundred: hundred, fifty: fifty, twenty: twenty, ten: ten)

// 금액이 부족하므로 최종적으로 false를 반환하여 처리하지 못함.
atm.withdraw(amount: 310)
// 100 화폐 가치에서 처리가 완료되어 true를 반환함.
atm.withdraw(amount: 100)
```

## Command

#### 명령 패턴

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

## Interpreter

#### 해석자 패턴

> 해석자 패턴은 한 언어에서 문장을 평가하기 위해 사용된다.

언어의 문법 또는 표현식을 평가하는 패턴이다.

### 코드

```swift
// 정수 표현식을 추상화한 프로토콜.
protocol IntegerExpression {
  // 정수 컨텍스트 평가.
  func evaluate(_ context: IntegerContext) -> Int
  func replace(character: Character, integerExpression: IntegerExpression) -> IntegerExpression
  func copied() -> IntegerExpression
}

// 정수 컨텍스트 클래스.
// Character를 키로, Int를 값으로 갖는 딕셔너리를 프로퍼티를 가지고 있으며,
// 이 딕셔너리에 대한 접근 및 할당 위한 메소드를 정의한다.
final class IntegerContext {
  private var data: [Character: Int] = [:]
  
  func lookup(name: Character) -> Int {
    return data[name]!
  }
  
  func assign(expression: IntegerVariableExpression, value: Int) {
    data[expression.name] = value
  }
}

// 정수 표현식 프로토콜을 채택하는 클래스.
final class IntegerVariableExpression: IntegerExpression {
  let name: Character
  
  init(name: Character) {
    self.name = name
  }
  
  func evaluate(_ context: IntegerContext) -> Int {
    return context.lookup(name: name)
  }
  
  func replace(character name: Character, integerExpression: IntegerExpression) -> IntegerExpresion {
    if name == self.name {
      return integerExpression.copied()
    } else {
      return IntegerVariableExpression(name: self.name)
    }
  }
  
  func copied() -> IntegerExpression {
    return IntegerVariableExpression(name: name)
  }
}

// 정수 표현식 프로토콜을 채택하는 더하기 표현식 클래스.
final class AddExpression: IntegerExpression {
  private var operand1: IntegerExpression
  private var operand2: IntegerExpression
  
  init(op1: IntegerExpression, op2: IntegerExpression) {
    operand1 = op1
    operand2 = op2
  }
  
  // 평가 메소드를 통해 더하기 연산을 수행.
  func evaluate(_ context: IntegerContext) -> Int {
    return operand1.evaluate(context) + operand2.evaluate(context)
  }
  
  func replace(character: Character, integerExpression: IntegerExpression) -> IntegerExpression {
    return AddExpression(op1: operand1.replace(character: character, integerExpression: integerExpression), op2: operand2.replace(character: character, integerExpression: integerExpression))
  }
  
  func copied() -> IntegerExpression {
    return AddExpression(op1: operand1, op2: operand2)
  }
}

/* 사용 */

var context = IntegerContext()

var a = IntegerVariableExpression(name: "A")
var b = IntegerVariableExpression(name: "B")
var c = IntegerVariableExpression(name: "C")

// a + (b + c)
var expression = AddExpression(op1: a, op2: AddExpression(op1: b, op2: c))

context.assign(expression: a, value: 2)
context.assign(expression: b, value: 1)
context.assign(expression: c, value: 3)

var result = expression.evaluate(context)
```

## Iterator

#### 반복자 패턴

> 반복자 패턴은 기반 구조를 이해할 필요 없이 집계 객체의 항목 모음을 탐색하기 위한 표준 인터페이스를 제공하기 위해 사용된다.

내부를 알 필요 없이 구성된 자료 구조를 탐색할 수 있게 한다.

컨테이너로부터 알고리즘을 분리시킨다.

컬렉션의 인덱스 접근에 의한 예외, 요소의 개수 등에 신경쓰지 않아도 되게 된다.

### 코드

Swift가 제공하는 IteratorProtocol, Sequence 프로토콜을 활용할 수 있다.

- IteratorProtocol
  - 한 번에 하나씩 시퀀스의 값을 제공하는 타입
  - Sequence 프로토콜과 밀접한 관계가 있음
    - Sequence 프로토콜의 Iterator 연관 타입은 IteratorProtocol을 채택하는 타입이어야 한다.
- Sequence
  - 요소에 순차적이고 반복적인 접근을 제공하는 타입

IteratorProtocol 프로토콜은 iterator에 의해 탐색되는 요소의 타입인 `Element` 를 연관 타입으로 갖고, 다음 요소를 탐색하는 `next()` 메소드를 요구한다.

Swift에서 `for-in` 반복문을 사용하려면 Sequence 프로토콜을 채택해야 한다.

결과적으로 `Array<Novella>` 가 아닌 반복자 패턴을 활용하여 `Novellas` 타입을 순회할 수 있도록 코드를 작성한다.

```swift
struct Novella {
  let name: String
}

struct Novellas {
  let novellas: [Novella]
}

// 반복자 패턴을 구현하기 위한 타입
struct NovellasIterator: IteratorProtocol {
  private var current = 0
  private let novellas: [Novella]
  
  init(novellas: [Novella]) {
    self.novellas = novellas
  }
  
  mutating func next() -> Novella? {
    defer {
      current += 1
    }
    return novellas.count > current ? novellas[current] : nil
  }
}

extension Novellas: Sequence {
  func makeIterator() -> NovellasIterator {
    return NovellasIterator(novellas: novellas)
  }
}

/* 사용 */

let greatNovellas = Novellas(novellas: [Novella(name: "The Mist")])

for novella in greatNovellas {
  print("I've read: \(novella)")
}
```

## Mediator

####  중재자 패턴

> 중재자 패턴은 서로 상호 작용하는 클래스 간 결합도를 줄이기 위해 사용된다. 직접적으로 상호 작용하는 클래스 대신, 그들의 구현에 대한 지식이 필요하므로 중재자 객체를 통해 메세지를 전달한다.

객체들의 집합이 상호 작용하는 방식을 함축한 객체를 정의하며, 이것이 중재자 객체가 된다.

객체 간 의존성을 줄여 결합도를 감소시킨다.

런타임 시의 행위를 중재자 객체에 묶어둔다.

프로그램에 존재하는 수많은 클래스 간 상호 작용을 분리하여 유지 보수성을 높인다.

### 코드

수신자를 추상화한 Receiver 프로토콜, 전송자를 추상화한 Sender 프로토콜을 정의한다.

Sender를 채택하는 중재자 클래스를 정의하고, 해당 객체를 사용하여 클래스 간 상호 작용을 구현한다.

```swift
protocol Receiver {
  associatedType MessageType
  func receive(message: MessageType)
}

protocol Sender {
  associatedType MessageType
  associatedType ReceiverType: Receiver
  var recipients: [ReceiverType] { get }
  func send(message: MessageType)
}

struct Programmer: Receiver {
  let name: String
  
  func receive(message: String) {
    print("\(name) received: \(message)")
  }
}

final class MessageMediator: Sender {
  var recipients: [Programmer] = []
  
  func add(recipient: Programmer) {
    recipients.append(recipient)
  }
  
  func send(message: String) {
    for recipient in recipients {
      recipient.receive(message: message)
    }
  }
}

/* 사용 */

// 중재자 객체를 참조하여 객체 간 상호 작용을 실현한다.
func spamMonster(message: String, worker: MessageMediator) {
  worker.send(message: message)
}

let messagesMediator = MessageMediator()

let user0 = Programmer(name: "Linus Torvalds")
let user1 = Programmer(name: "Avadis 'Avie' Tevanian")
messagesMediator.add(recipient: user0)
messagesMediator.add(recipient: user1)

spamMonster(message: "I'd Like to Add you to My Professional Network", worker: messagesMediator)
```

## Memento

#### 메멘토 패턴

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

## Observer

#### 옵저버 패턴

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

## State

#### 상태 패턴

> 상태 패턴은 내부 상태가 변경될 때 객체의 행위를 변경하기 위해 사용된다. 이 패턴은 객체의 클래스가 런타임에 변경될 수 있도록 한다.

정보 객체를 상태 객체로 넘겨 상태 객체마다 다른 정보를 담는다.

객체 지향 방식으로 상태 기계를 구현한다.

상태 패턴 인터페이스의 파생 클래스로 각각의 상태를 구현한다.

런타임에서 객체의 행위를 손쉽게 바꿀 수 있다.

### 코드

컨텍스트 객체를 상태 객체로 넘겨 해당 상태를 가져온다.

```swift
// 컨텍스트 객체가 상태를 담는다.
final class Context {
  private var state: State = Unauthorized()
  
  var isAuthorized: Bool {
    return state.isAuthorized(context: self)
  }
  
  func changeStateToAuthorized(userID: String) {
    state = AuthorizedState(userID: userID)
  }
  
  func changeStateToUnauthorized() {
    state = UnauthorizedState()
  }
}

// 상태 인터페이스 정의.
protocol State {
  func isAuthorized(context: Context) -> Bool
  func userID(context: Context) -> String?
}

// 상태 인터페이스를 구현하는 각각의 상태 정의.
class UnauthorizedState: State {
  func isAuthorized(context: Context) -> Bool {
    return false
  }
  
  func userID(context: Context) -> String? {
    return nil
  }
}

// 상태 인터페이스를 구현하는 각각의 상태 정의.
class AuthorizedState: State {
  let userID: String
  
  init(userID: String) {
    self.userID = userID
  }
  
  func isAuthorized(context: Context) -> Bool {
    return true
  }
  
  func userID(context: Context) -> String? {
    return userID
  }
}

/* 사용 */

let userContext = Context()
(userContext.isAuthorized, userContext.userID)
userContext.changeStateToAuthorized(userID: "admin")
(userContext.isAuthorized, userContext.userID)
userContext.changeStateToUnauthorized()
(userContext.isAuthorized, userContext.userID)
```

## Strategy

#### 전략 패턴

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

## Visitor

#### 방문자 패턴

> 방문자 패턴은 상대적으로 복잡한 구조화된 데이터 클래스 집합을 보유하고 있는 데이터에서 수행할 수 있는 기능과 분리하기 위해 사용된다.

알고리즘을 객체 구조에서 분리시킨다. 구조를 수정하지 않고도 새로운 동작을 기존의 객체 구조에 추가할 수 있게 된다.

OCP를 적용하는 방법 중 하나이다.

### 코드

행성을 추상화한 Planet 프로토콜을 정의하고 이를 채택하는 각각의 구체 행성 클래스를 만든다.

위에서 정의한 데이터에서 수행할 수 있는 기능을 분리한다.

```swift
protocol PlanetVisitor {
  func visit(planet: PlanetAlderaan)
  func visit(planet: PlanetCoruscant)
  func visit(planet: PlanetTatooine)
  func visit(planet: MoonJedha)
}

protocol Planet {
  func accept(visitor: PlanetVisitor)
}

final class MoonJedha: Planet {
  func accept(visitor: PlanetVisitor) {
    visitor.visit(planet: self)
  }
}

final class PlanetAlderaan: Planet {
  func accept(visitor: PlanetVisitor) {
    visitor.visit(planet: self)
  }
}

final class PlanetCoruscant: Planet {
  func accept(visitor: PlanetVisitor) {
    visitor.visit(planet: self)
  }
}

final class PlanetTatoonie: Planet {
  func accept(visitor: PlanetVisitor) {
    visitor.visit(planet: self)
  }
}

final class NameVisitor: PlanetVisitor {
  var name = ""
  
  func visit(planet: PlanetAlderaan) {
    name = "Alderaan"
  }
  
  func visit(planet: PlanetCoruscant) {
    name = "Coruscant"
  }
  
  func visit(planet: PlanetTatooine) {
    name = "Tatooine"
  }
  
  func visit(planet: MoonJedha) {
    name = "Jedha"
  }
}

/* 사용 */

let planets: [Planet] = [PlanetAlderaan(), PlanetCoruscant(), PlanetTatooine(), MoonJedha()]

let names = planets.map { planet in
  let visitor = NameVisitor()
  planet.accept(visitor: visitor)
  return visitor.name
}

names
```



