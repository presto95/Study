# Abstract Factory

### 추상 팩토리 패턴

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