# Creational

#### 생성 패턴

> 소프트웨어 공학에서, 생성 패턴은 객체 생성 매커니즘을 다루는 디자인 패턴이며, 상황에 맞는 방식으로 객체를 생성한다. 객체 생성의 기본 형식은 설계 문제나 설계의 복잡성 증가의 결과를 가져올 수 있다. 생성 디자인 패턴은 객체 생성을 어떠한 방식으로 제어하여 이 문제를 해결한다.

객체 생성과 관련된 패턴.

## Abstract Factory

#### 추상 팩토리 패턴

> 추상 팩토리 패턴은 클라이언트에게 관계 있거나 의존적인 객체 집합을 제공하기 위해 사용된다. 팩토리에 의해 생성된 객체 "패밀리"는 런타임에서 결정된다.

객체 생성을 관리하는 클래스를 따로 두도록 한다.

객체를 생성하는 팩토리를 만드는데, 객체가 다양해지면서 팩토리도 많아질 것이기 때문에 팩토리를 추상화한다.

다양한 구성 요소 별로 객체의 집합을 생성해야 할 때 유용하다. 상황에 알맞은 객체를 생성할 수 있다.

### 코드

Swift의 열거형을 활용하여 추상 팩토리를 정의한다.

```swift
protocol BurgerDescribing {
  var ingredients: [String] { get }
}

struct CheeseBurger: BurgerDescribing {
  let ingredients: [String]
}

protocol BurgerMaking {
  func make() -> BurgerDescribing
}

final class BigKahunaBurger: BurgerMaking {
  func make() -> BurgerDescribing {
    return CheeseBurger(ingredients: ["Cheese", "Burger", "Lettuce", "Tomato"])
  }
}

final class JackInTheBox: BurgerMaking {
  func make() -> BurgerDescribing {
    return CheeseBurger(ingredients: ["Cheese", "Burger", "Tomato", "Onions"])
  }
}

/* 추상 팩토리 */

enum BurgerFactoryType: BurgerMaking {
  case bigKahuna
  case jackInTheBox
  
  func make() -> BurgerDescribing {
    switch self {
    case .bigKahuna:
      return BigKahunaBurger().make()
    case .jackInTheBox:
      return JackInTheBox().make()
    }
  }
}

/* 사용 */

let bigKahuna = BurgerFactoryType.bigKahuna.make()
let jackInThebox = BurgerFactoryType.jackInTheBox.make()
```

## Builder

#### 빌더 패턴

> 빌더 패턴은 복합 객체들을 같은 순서 또는 특정 알고리즘을 사용하여 생성되어야 하는 구성 요소로 생성하기 위해 사용된다. 외부 클래스는 생성 알고리즘을 제어한다.

복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 한다.

각 특징을 갖는 클래스를 모아서 객체를 생성한다.

### 추상 메소드 패턴과의 차이

- 추상 메소드 패턴은 Product를 알아야 한다.
- 빌더 패턴은 Product를 알 필요가 없다. 사용자는 빌더 객체를 통해 명령을 내리며 빌더가 알아서 작업을 수행한다.
- 빌더 패턴은 Product에 대한 의존이 모두 빌더 객체에 위치한다.

### 코드

```swift
final class DeathStarBuilder {
  var x: Double?
  var y: Double?
  var z: Double?
  
  typealias BuilderClosure = (DeathStarBuilder) -> Void
  
  init(buildClosure: BuilderClosure) {
    buildClosure(self)
  }
}

struct DeathStar {
  let x: Double
  let y: Double
  let z: Double
  
  init?(builder: DeathStarBuilder) {
    if let x = builder.x, let y = builder.y, let z = builder.z {
      self.x = x
      self.y = y
      self.z = z
    } else {
      return nil
    }
  }
}

/* 사용 */

let empire = DeathStarBuilder { builder in 
  builder.x = 0.1
  builder.y = 0.2
  builder.z = 0.3
}

let deathStar = DeathStar(builder: empire)
```

## Factory Method

#### 팩토리 메소드 패턴

> 팩토리 메소드 패턴은 인스턴스화된 객체의 타입이 런타임에서 결정될 수 있도록 객체 생성 프로세스를 추상화하여 클래스의 생성자를 대체하기 위해 사용된다.

단순히 객체를 생성하는 메소드를 일컫는 것이 아니다.

상위 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴. 자식 클래스가 어떤 객체를 생성할지 결정하도록 한다.

### 코드

열거형에 객체 생성을 위한 팩토리 메소드를 정의하였다.

```swift
protocol CurrencyDescribing {
  var symbol: String { get }
  var code: String { get }
}

final class Euro: CurrencyDescribing {
  var symbol: String {
    return "€"
  }
  var code: String {
    return "EUR"
  }
}

final class UnitedStatesDollar: CurrencyDescribing {
  var symbol: String {
    return "$"
  }
  var code: String {
    return "USD"
  }
}

enum Country {
  case unitedStates
  case spain
  case uk
  case greece
}

enum CurrencyFactory {
  static func currency(for country: Country) -> CurrencyDescribing? {
    switch country {
    case .spain, .greece:
      return Euro()
    case .unitedStates:
      return UnitedStatesDollar()
    default:
      return nil
    }
  }
}

/* 사용 */

let noCurrencyCode = "No Currency Code Available"

CurrencyFactory.currency(for: .greece)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .spain)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .unitedStates)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .uk)?.code ?? noCurrencyCode
```

## Prototype

#### 프로토타입 패턴

> 프로토타입 패턴은 존재하는 모든 프로퍼티를 복사하여 새로운 객체를 생성하여 독립된 복제본을 만들기 위해 사용된다. 이 방법은 특히 새로운 객체의 생성이 비효율적일 때 유용하다.

생성자의 인자가 고정되는 경우라면 초기 생성을 프로토타입으로 하여 생성될 객체를 복제한다.

생성할 객체들의 타입이 프로토타입인 인스턴스로부터 결정되도록 하며, 인스턴스는 새 객체를 만들기 위해 자신을 복제하게 된다.

일반적으로 `clone()` 메소드를 정의하여 패턴을 구현한다.

### 코드

공통된 `name` 프로퍼티를 갖는 여러 개의 객체를 생성해야 하는 경우 프로토타입 객체를 만들고 그것으로부터 인스턴스를 복제할 수 있도록 한다.

```swift
struct MoonWorker {
  let name: String
  var health: Int = 100
  
  init(name: String) {
    self.name = name
  }
  
  func clone() -> MoonWorker {
    return MoonWorker(name: name)
  }
}

/* 사용 */

let prototype = MoonWorker(name: "Sam Bell")

var bell1 = prototype.clone()
bell1.health = 12

var bell2 = prototype.clone()
bell2.health = 23

var bell3 = prototype.clone()
bell3.health = 0
```

## Singleton

#### 싱글턴 패턴

> 싱글턴 패턴은 특정 클래스에 대하여 오직 하나의 객체만 생성되는 것을 보장한다. 싱글턴 클래스에 대한 모든 이후 참조들은 같은 기반 인스턴스를 참조한다. 애플리케이션이 거의 없고, 이 패턴을 과도하게 사용하지 마라.

생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이며, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 반환한다.

애플리케이션에서 오직 하나의 객체만 존재하는 것을 보장한다.

### 코드

Swift에서는 일반적으로 `shared` 프로퍼티를 정의하여 싱글턴 객체를 참조한다.

```swift
final class ElonMusk {
  static let shared = ElonMusk()
  
  // private 접근 수준을 갖는 이니셜라이저를 정의하여 사용 시점에서 이니셜라이저를 호출하지 못하도록 한다.
  private init() { }
}

/* 사용 */

let elon = ElonMusk.shared
```