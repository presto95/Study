# Builder

### 빌더 패턴

> 빌더 패턴은 복합 객체들을 같은 순서 또는 특정 알고리즘을 사용하여 생성되어야 하는 구성 요소로 생성하기 위해 사용된다. 외부 클래스는 생성 알고리즘을 제어한다.

복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 한다.

각 특징을 갖는 클래스를 모아서 객체를 생성한다.

### 추상 팩토리 패턴과의 차이

- 추상 팩토리 패턴은 Product를 알아야 한다.
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

---

일반적으로 아래와 같은 형태를 띈다.

```swift
final class ThreeDimension {
  var x: Double = 0
  var y: Double = 0
  var z: Double = 0
}

final class ThreeDimensionBuilder {
  
  private let dimension = ThreeDimension()
  
  func withX(_ x: Double) -> ThreeDimensionBuilder {
    dimension.x = x
    return self
  }
  
  func withY(_ y: Double) -> ThreeDimensionBuilder {
    dimension.y = y
    return self
  }
  
  func withZ(_ z: Double) -> ThreeDimensionBuilder {
    dimension.z = z
    return self
  }
  
  func build() -> ThreeDimension {
    return dimension
  }
}

ThreeDimensionBuilder()
  .withX(1)
  .withY(1)
  .withZ(1)
  .build()
```

메소드 체이닝을 통해 멤버 변수를 채워 나간다.

어떠한 타입을 인스턴스화할 때 필요한 인자의 개수가 많은 경우에도 유용하게 사용될 수 있다.

Swift의 extension 기능을 활용하여 빌더 클래스를 따로 정의하지 않고도 비슷하게 구현할 수 있겠다.

```swift
final class ThreeDimension {
  var x: Double = 0
  var y: Double = 0
  var z: Double = 0
}

extension ThreeDimension {
  
  static func make() -> ThreeDimension {
    return .init()
  }
  
  func withX(_ x: Double) -> ThreeDimension {
    self.x = x
    return self
  }
  
  func withY(_ y: Double) -> ThreeDimension {
    self.y = y
    return self
  }
  
  func withZ(_ z: Double) -> ThreeDimension {
    self.z = z
    return self
  }
}

ThreeDimension.make()
  .withX(1)
  .withY(1)
  .withZ(1)
```

내가 자주 쓰는 UIAlertController extension이 위와 비슷한 형태로 구현되어 있다.

```swift
extension UIAlertController {
  
  static func alert(title: String?,
                    message: String?,
                    style: UIAlertController.Style = .alert) -> UIAlertController {
    return .init(title: title, message: message, preferredStyle: style)
  }
  
  @discardableResult
  func textField(configurationHandler: ((UITextField) -> Void)? = nil) -> UIAlertController {
    addTextField(configurationHandler: configurationHandler)
    return self
  }
  
  @discardableResult
  func action(title: String?,
              style: UIAlertAction.Style = .default,
              handler: ((UIAlertAction, [UITextField]?) -> Void)? = nil) -> UIAlertController {
    let action = UIAlertAction(title: title, style: style) { action in
      handler?(action, self.textFields)
    }
    addAction(action)
    return self
  }
  
  func present(to viewController: UIViewController,
               animated: Bool = true,
               completion: (() -> Void)? = nil) {
    viewController.present(self, animated: animated, completion: completion)
  }
}
```

