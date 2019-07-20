# Builder

### 빌더 패턴

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