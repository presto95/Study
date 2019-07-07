# Behavioral

#### 행위 / 동작 패턴

> 소프트웨어 공학에서, 동작 패턴은 오브젝트 간 일반적인 상호 작용 패턴을 식별하고 그러한 것을 구현하는 디자인 패턴이다. 이렇게 하는 것으로 상호 작용을 하는 데 있어서 유연성을 증가시킨다.

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

### 코드

Swift가 제공하는 IteratorProtocol을 활용한다.

```swift
struct Novella {
  let name: String
}

struct Novellas {
  let novellas: [Novella]
}

struct NovellasIterator: IteratorProtocol {
  
}
```

