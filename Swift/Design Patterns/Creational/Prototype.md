# Prototype

### 프로토타입 패턴

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