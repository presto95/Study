# Interpreter

### 해석자 패턴

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