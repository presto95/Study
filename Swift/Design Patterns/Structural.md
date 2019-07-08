# Structural

#### 구조 패턴

> 소프트웨어 공학에서, 구조 디자인 패턴은 엔티티 간 관계를 실현하는 간단한 방법을 식별하여 설계를 쉽게 수행하게 하는 디자인 패턴이다.

클래스 간 구조를 의미론적으로 더욱 명확하게 하고 확장성을 좋게 한다.

## Adapter

#### 어댑터 패턴

> 어댑터 패턴은 클라이언트가 요구하는 인터페이스를 지원하는 클래스로 "adaptee"를 래핑하여 다른 두 가지 호환되지 않는 타입 간의 연결을 제공하기 위해 사용된다.

클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환한다.

호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 연결될 수 있도록 한다.

### 코드

Old 인터페이스와 New 인터페이스가 있는데, Old 'adaptee'를 래핑하여 두 개의 인터페이스를 연결시켰다.

```swift
protocol NewDeathStarSuperLaserAiming {
  var angleV: Double { get }
  var angleH: Double { get }
}

/* Adaptee */

struct OldDeathStarSuperLaserTarget {
  let angleHorizontal: Float
  let angleVertical: Float
}

/* Adapter */

struct NewDeathStarSuperLaserTarget: NewDeathStarSuperLaserAiming {
  private let target: OldDeathStarSuperLaserTarget
  
  init(target: OldDeathStarSuperLaserTarget) {
    self.target = target
  }
  
  var angleV: Double {
    return Double(target.angleVertical)
  }
  var angleH: Double {
    return Double(target.angleHorizontal)
  }
}

/* 사용 */

let target = OldDeathStarSuperLaserTarget(angleHorizontal: 14, angleVertical: 12)
let newFormat = NewDeathStarSuperLaserTarget(target)

newFormat.angleH
newFormat.angleV
```

## Bridge

#### 브릿지 패턴

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

## Composite

#### 컴포지트 패턴

> 컴포지트 패턴은 관계된 객체들의 계층적이고 재귀적인 트리 자료 구조를 만들기 위해 사용되며, 자료 구조의 요소는 표준 방식으로 접근되고 활용될 수 있다.

객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현한다. 

클라이언트가 단일 객체와 복합 객체 모두 동일하게 다룰 수 있다.

- Component : 모든 표현할 요소의 추상화된 인터페이스
- Leaf : Component 구현
- Composite : Component를 구현함과 동시에 Component를 여러 개 가질 수 있음

파일 시스템을 구현한다고 할 때, 파일을 추상화한 것을 Component, 이를 구현한 것을 Leaf, Component를 여러 개 가져 디렉토리의 개념을 구현하는 것을 Composite라고 말할 수 있다.

### 코드

```swift
/* Component */

protocol Shape {
  func draw(fillColor: String)
}

/* Leafs */

final class Square: Shape {
  func draw(fillColor: String) {
    print("Drawing a Square with color \(fillColor)")
  }
}

final class Circle: Shape {
  func draw(fillColor: String) {
    print("Drawing a Circle with color \(fillColor)")
  }
}

/* Composite */

final class Whiteboard: Shape {
  private lazy var shapes = [Shape]()
  
  init(shapes: Shape...) {
    self.shapes = shapes
  }
  
  func draw(fillColor: String) {
    for shape in shapes {
      shape.draw(fillColor: fillColor)
    }
  }
}

/* 사용 */

var whiteboard = WhiteBoard(Circle(), Square())
whiteboard.draw(fillColor: "Red")
```

## Decorator

#### 데코레이터 패턴

> 데코레이터 패턴은 객체들의 기능을 데코레이터 클래스 객체에 감싸 런타임에 그것들을 확장하거나 변경하기 위해 사용된다. 이는 행위를 변경하는 것에 있어서 상속을 대신하는 유연한 대안을 제공합니다.

주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙인다.

기능 확장이 필요할 때 서브클래싱 대신 사용할 수 있는 유연한 대안이 될 수 있다.

### 코드

```swift
protocol CostHaving {
  var cost: Double { get }
}

protocol IngredientsHaving {
  var ingredients: [String] { get }
}

typealias BeverageDataHaving = CostHaving & IngredientsHaving

struct SimpleCoffee: BeverageDataHaving {
  let cost: Double = 1
  let ingredients: [String] = ["Water", "Coffee"]
}

protocol BeverageHaving: BeverageDataHaving {
  // 서브클래싱 대신 해당 기능을 갖는 타입을 추상화.
  var beverage: BeverageDataHaving { get }
}

struct Milk: BeverageHaving {
  let beverage: BeverageDataHaving
  
  var cost: Double {
    return beverage.cost + 0.5
  }
  var ingredients: [String] {
    return beverage.ingredients + ["Milk"]
  }
}

struct WhipCoffee: BeverageHaving {
  let beverage: BeverageDataHaving
  
  var cost: Double {
    return beverage.cost + 0.5
  }
  var ingredients: [String] {
    return beverage.ingredients + ["Whip"]
  }
}

/* 사용 */

var someCoffee: BeverageDataHaving = SimpleCoffee()
someCoffee = Milk(beverage: someCoffee)
someCoffee = WhipCoffee(beverage: someCoffee)
```

## Façade

#### 파사드 패턴

> 파사드 패턴은 복합적인 서브시스템에 단순화된 인터페이스를 정의하기 위해 사용된다.

다른 구체 클래스를 직접 가지고 있어 여러 개의 모듈을 하나로 모아 쉽고 단순한 인터페이스를 제공한다.

여러 인터페이스를 통합한 하나의 인터페이스를 제공한다.

### 데코레이터 패턴과의 차이

데코레이터 패턴은 서브클래싱이 아닌 방법을 사용하여 특정 클래스의 기능을 확장한다.

데코레이터 패턴은 런타임에 기능 확장이 필요할 때 사용하며, 파사드 패턴은 컴파일 타임에 기능 확장이 이루어진다.

### 어댑터 패턴과의 차이

어댑터 패턴은 서로 다른 특징을 가진 클래스를 연결한다.

기능을 가진 클래스가 구체 클래스가 아니라면 클래스 어댑터 패턴을 사용한다.

파사드 패턴은 구체 클래스를 직접 가지고 있다.

### 코드

```swift
final class Defaults {
  private let defaults: UserDefaults
  
  init(defaults: UserDefaults = .default) {
    self.defaults = defaults
  }
  
  subscript(key: String) -> String? {
    get {
      return defaults.string(forKey: key)
    }
    set {
      defaults.set(newValue, forKey: key)
    }
  }
}

/* 사용 */

let storage = Defaults()
storage["Bishop"] = "Disconnect me. I'd rather be nothing"
storage["Bishop"]
```

## Flyweight

##### 플라이웨이트 패턴

> 플라이웨이트 패턴은 비슷한 객체들을 최대한 많이 공유하여 메모리 사용이나 컴퓨터 자원 낭비를 최소화하기 위해 사용된다.

프로그램 내에서 사용되는 데이터들을 모아 공유한다.

공유되므로, 불변성 및 대등 (Immutability / Equality) 에 신경써야 한다.

### 코드

```swift
struct SpecialityCoffee {
  let origin: String
}

protocol CoffeeSearching {
  func search(origin: String) -> SpecialityCoffee?
}

final class Menu: CoffeeSearching {
  private var coffeeAvailable: [String: SpecialityCoffee] = [:]
  
  func search(origin: String) -> SpecialityCoffee? {
    if coffeeAvailable.index(forKey: origin) == nil {
      coffeeAvailable[origin] = SpecialityCoffee(origin: origin)
    }
    return coffeeAvailable[origin]
  }
}

final class CoffeeShop {
  private var orders: [Int: SpecialityCoffee] = [:]
  private let menu: CoffeeSearching
  
  init(menu: CoffeeSearching) {
    self.menu = menu
  }
  
  func takeOrder(origin: String, table: Int) {
    orders[table] = menu.search(origin: origin)
  }
  
  func serve() {
    for (table, origin) in orders {
      print("Serving \(origin) to table \(table)")
    }
  }
}

/* 사용 */

let coffeeShop = CoffeeShop(menu: Menu())

coffeeShop.takeOrder(origin: "Yirgacheffe, Ethiopia", table: 1)
coffeeShop.takeOrder(origin: "Buziraguhindwa, Burundi", table: 3)

coffeeShop.serve()
```

