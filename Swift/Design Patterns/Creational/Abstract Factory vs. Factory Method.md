# Abstract Factory vs. Factory Method

컴퓨터는 마우스와 키보드를 가지고 있고, 타입에 따라 다른 컴퓨터를 만들어야 하는 요구 사항이 있다고 가정하자. 타입은 A 타입과 B 타입이 있다.

```swift
enum Type {
  case a
  case b
}
```

**추상 팩토리 패턴**을 사용하면 다음과 같은 코드를 작성하여 구현할 수 있다.

```swift
protocol Keyboard { }
final class AKeyboard: Keyboard { }
final class BKeyboard: Keyboard { }

protocol Mouse { }
final class AMouse: Mouse { }
final class BMouse: Mouse { }

protocol ComputerFactory {
  var keyboard: Keyboard { get }
  var mouse: Mouse { get }
}
final class AComputerFactory: ComputerFactory {
  var keyboard: Keyboard {
    return AKeyboard()
  }
  var mouse: Mouse {
    return AMouse()
  }
}
final class BComputerFactory: ComputerFactory {
  var keyboard: Keyboard {
    return BKeyboard()
  }
  var mouse: Mouse {
    return BMouse()
  }
}

final class Computer {
  let keyboard: Keyboard
  let mouse: Mouse
  init(keyboard: Keyboard, mouse: Mouse) {
    self.keyboard = keyboard
    self.mouse = mouse
  }
}

// Usage

let aFactory = AComputerFactory()
let bFactory = BComputerFactory()
let aComputer = Computer(keyboard: aFactory.keyboard, mouse: aFactory.mouse)
let bComputer = Computer(keyboard: bFactory.keyboard, mouse: bFactory.mouse)
```

구성품을 모아 하나의 객체를 만드는 형식이다.

키보드 구성품을 추상화한 후 각 타입의 키보드를 만든다.

마우스 구성품을 추상화한 후 각 타입의 마우스를 만든다.

컴퓨터 팩토리를 추상화하며, 이는 구성품을 모으는 역할을 하므로 키보드와 마우스를 요구한다.

각 타입의 컴퓨터에 대한 팩토리를 만든다.

만들어진 팩토리를 사용하여 컴퓨터를 만든다.

---

**팩토리 메소드 패턴**을 사용하면 다음과 같은 코드를 작성하여 구현할 수 있다.

```swift
protocol Keyboard { }
final class AKeyboard: Keyboard { }
final class BKeyboard: Keyboard { }
final class KeyboardFactory {
  func make(_ type: Type) -> Keyboard {
    switch type {
    case .a:
      return AKeyboard()
    case .b:
      return BKeyboard()
    }
  }
}

protocol Mouse { }
final class AMouse: Mouse { }
final class BMouse: Mouse { }
final class MouseFactory {
  func make(_ type: Type) -> Mouse {
    switch type {
    case .a:
      return AMouse()
    case .b:
      return BMouse()
    }
  }
}

final class Computer {
  let keyboard: Keyboard
  let mouse: Mouse
  init(keyboard: Keyboard, mouse: Mouse) {
    self.keyboard = keyboard
    self.mouse = mouse
  }
}

final class ComputerFactory {
  func make(_ type: Type) -> Computer {
    let keyboard = KeyboardFactory().make(type)
    let mouse = MouseFactory().make(type)
    return Computer(keyboard: keyboard, mouse: mouse)
  }
}

// Usage

let factory = ComputerFactory()
let aComputer = factory.make(.a)
let bComputer = factory.make(.b)

```

구성품을 만드는 팩토리를 정의하며, 타입에 따라 분기하여 하나의 메소드에서 각기 다른 객체를 반환한다.

---

추상 팩토리 패턴의 경우 다른 타입이 생기는 경우에도 코드의 수정 없이 클래스를 추가하는 것으로 변경된 요구 사항을 실현할 수 있으나, 팩토리 메소드 패턴의 경우 하나의 메소드에서 타입으로 분기하여 각기 다른 객체를 반환하는 형식이므로 코드의 수정이 불가피하다.