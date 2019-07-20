# Decorator

### 데코레이터 패턴

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