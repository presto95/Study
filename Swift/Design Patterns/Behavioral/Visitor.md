# Visitor

### 방문자 패턴

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