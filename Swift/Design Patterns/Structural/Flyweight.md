# Flyweight

### 플라이웨이트 패턴

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