[The Swift Programming Language - Language Guide - Opaque Types](https://docs.swift.org/swift-book/LanguageGuide/OpaqueTypes.html)

# 불투명 타입

불투명 반환 타입을 갖는 함수나 메소드는 그 반환 값의 타입 정보를 숨긴다. 함수의 반환 타입으로 구체적인 타입을 제공하는 대신, 반환 값은 그것이 지원하는 프로토콜의 관점에서 나타내어진다. 타입 정보를 숨기는 것은 모듈과 모듈 내부에서 호출하는 코드 사이의 경계에서 유용한데, 반환 값의 기반 타입이 비공개 접근 수준일 수 있기 때문이다. 프로토콜 타입을 갖는 값을 반환하는 것과 달리, 불투명 타입은 타입 동일성을 보존한다. 컴파일러는 타입 정보에 접근하지만, 모듈의 클라이언트는 그렇지 않는다.

## 불투명 타입이 해결하는 문제

예를 들어 아스키 아트 도형을 그리는 모듈을 작성하고 있다고 가정하자. 아스키 아트 도형의 기본 특징은 도형의 문자열 표현을 반환하는 `draw()` 함수이며, `Shape` 프로토콜의 요구 조건으로서 사용할 수 있다.

```swift
protocol Shape {
  func draw() -> String
}

struct Triangle: Shape {
  var size: Int
  func draw() -> String {
    var result = [String]()
    for length in 1...size {
      result.append(String(repeating: "*", count: length))
    }
    return result.joined(separator: "\n")
  }
}

let smallTriangle = Triangle(size: 3)
smallTriangle.draw()
```

도형을 수직으로 플립하는 것과 같은 동작을 구현하기 위해 아래의 코드처럼 제네릭을 사용할 수 있을 것이다. 그러나 이 접근에는 중요한 한계가 있는데, 플립된 결과는 그것을 생성하기 위해 사용한 정확한 제네릭 타입을 노출한다는 것이다.

```swift
struct FlippedShape<T: Shape>: Shape {
  var shape: T
  func draw() -> String {
    let lines = shape.draw().split(separator: "\n")
    return lines.reversed().joined(separator: "\n")
  }
}

let flippedTriangle = FlippedShape(shape: smallTriangle)
flippedTriangle.draw()
```

아래의 코드처럼 두 개의 도형을 수직으로 붙여서 `JoinedShape<T: Shape, U: Shape>` 구조체를 정의하는 접근법은 플립된 삼각형을 또다른 삼각형과 붙이는 것으로 `JoinedShape<FlippedShape<Triangle>, Triangle>` 과 같은 타입의 결과를 가져온다.

```swift
struct JoinedShape<T: Shape, U: Shape>: Shape {
  var top: T
  var bottom: U
  func draw() -> String {
    return top.draw() + "\n" + bottom.draw()
  }
}

let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
joinedTriangles.draw()
```

도형의 생성과 관련된 세부 정보를 노출하는 것은 아스키 아트 모듈의 공개 인터페이스의 일부분임을 의미하지 않는 타입이 완전한 반환 타입을 나타내는 필요에 의해 유출되도록 한다. 모듈 내부에 있는 코드는 다양한 방법으로 같은 도형을 만들어낼 수 있을 것이며, 그 도형을 사용하는 모듈 바깥에 있는 다른 코드는 변환 리스트와 관련된 세부 구현을 설명하지 않아야 할 것이다. `JoinedShape` 와 `FlippedShape` 와 같은 래퍼*wrapper* 타입은 모듈의 사용자에게 중요하지 않고, 보여지지 않아야 한다. 모듈의 공개 인터페이스는 도형을 붙이고 플립하는 것과 같은 동작들로 구성되어 있으며, 그러한 동작은 또다른 `Shape` 값을 반환한다.

## 불투명 타입 반환

불투명 타입을 제네릭 타입의 반대 개념으로 생각할 수 있다. 제네릭 타입은 함수를 호출하는 코드가 해당 함수의 매개 변수에 대한 타입을 선택하고 함수 구현으로부터 추상화된 방식으로 값을 반환하게 한다. 예를 들어 다음의 코드에 있는 함수는 호출자에 의존한 타입을 반환한다.

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

`max(_:_:)` 함수를 호출하는 코드는 `x` 와 `y` 를 위한 값을 선택하고, 그 값들의 타입은 `T` 의 구체적인 타입을 결정한다. 호출하는 코드는 `Comparable` 프로토콜을 준수하는 어떠한 타입이라도 사용할 수 있다. 함수 내에 있는 코드는 일반적인 방식으로 작성되어 호출자가 제공하는 어떠한 타입이라도 처리할 수 있게 된다. `max(_:_:)` 의 구현은 모든  `Comparable` 타입이 공유하는 기능만을 사용한다.

이 역할들은 함수에서 불투명 반환 타입을 사용하여 반전된다. 불투명 타입은 함수 구현이 함수를 호출하는 코드에서 추상화된 방식으로 반환하는 값의 타입을 선택하게 한다. 예를 들어 다음의 예제에 있는 함수는 도형의 기반 타입을 노출하지 않고 사다리꼴 도형을 반환한다.

```swift
struct Square: Shape {
  var size: Int
  func draw() -> String {
    let line = String(repeating: "*", count: size)
    let result = Array<String>(repeating: line, count: size)
    return result.joined(separator: "\n")
  }
}

func makeTrapezoid() -> some Shape {
  let top = Triangle(size: 2)
  let middle = Square(size: 2)
  let bottom = FlippedShape(shape: top)
  let trapezoid = JoinedShape(top: top, bottom: JoinedShape(top: middle, bottom: bottom))
  return trapezoid
}

let trapezoid = makeTrapezoid()
trapezoid.draw()
```

예제에서 `makeTrapezoid()` 함수는 반환 타입을 `some Shape` 로 선언한다. 그 결과로 함수는 `Shape` 프로토콜을 준수하는 어떠한 주어진 타입의 값을 반환하는데, 이는 특정한 구체적 타입을 지정하지 않는다. 이러한 방식으로 `makeTrapezoid()` 를 작성하여 공개 인터페이스의 일부분에서 만들어지는 도형의 특정 타입을 만들지 않고 그 공개 인터페이스의 근본적인 양상을 표현할 수 있도록 한다. (반환 값은 어떠한 도형이다.) 이 구현은 두 개의 삼각형과 하나의 정사각형을 사용하나, 함수는 그 반환 타입을 바꾸지 않고 다른 다양한 방식으로 사다리꼴을 그리기 위해 다시 작성될 수 있을 것이다.

이 예제는 불투명 반환 타입이 제네릭 타입의 역*reverse*과 같다는 방식에 집중한다. `makeTrapezoid()` 내의 코드는 그 타입이 `Shape` 프로토콜을 준수하는 한 그것이 요구하는 어떠한 타입이라도 반환할 수 있으며, 이는 호출 코드가 제네릭 함수에서 수행하는 것과 같다. 해당 함수를 호출하는 코드는 `makeTrapezoid()` 에 의해 반환되는 어떠한 `Shape` 값과 동작할 수 있게 제네릭 함수의 구현처럼 일반적인 방식으로 작성될 필요가 있다.

불투명 반환 타입을 제네릭과 조합할 수도 있다. 다음의 코드에 있는 함수는 모두 `Shape` 프로토콜을 준수하는 어떠한 타입의 값을 반환한다.

```swift
func flip<T: Shape>(_ shape: T) -> some Shape {
  return FlippedShape(shape: shape)
}

func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
  JoinedShape(top: top, bottom: bottom)
}

let opqueJoinedTriangles = join(smallTriangle, flip(smallTriangle))
opaqueJoinedTriangles.draw()
```

이 예제에 있는 `opaqueJoinedTriangles` 의 값은 이 챕터의 "불투명 타입이 해결하는 문제" 섹션 초반 제네릭 예제에 있는 `joinedTriangles` 와 같다. 그러나 그 예제의 값과는 다르게, `flip(_:)` 과 `join(_:_:)` 은 제네릭 도형 동작이 반환하는 기반 타입을 불투명 반환 타입으로 감싸며, 이는 그러한 타입이 보여지지 않도록 한다. 두 개의 함수 모두 그것들이 의존하는 타입이 제네릭이므로 일반적이며, 함수에 대한 타입 매개 변수는 `FlippedShape`와 `JoinedShape`가 요구하는 타입 정보를 전달한다.

만약 불투명 반환 타입을 갖는 함수가 여러 위치에서 반환한다면, 모든 가능한 반환 값은 같은 타입을 가져야 한다. 제네릭 함수에서, 그 반환 타입은 함수의 제네릭 타입 매개 변수를 사용할 수 있으나, 그것은 여전히 하나의 타입이어야 한다. 예를 들어 정사각형을 위한 특별 케이스를 포함하는 도형을 플립하는 함수의 *유효하지 않은* 버전이 있다.

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
  if shape is Square {
    return shape  // 에러: 반환 타입이 일치하지 않음
  }
  return FlippedShape(shape: shape)  // 에러: 반환 타입이 일치하지 않음
}
```

이 함수를 `Square` 와 함께 호출한다면 이것은 `Square` 를 반환할 것이며, 그렇지 않으면 `FlippedShape` 를 반환할 것이다. 이는 오직 한 가지 타입의 값만을 반환해야 한다는 요구 사항을 위반하고 `invalidFlip(_:)` 을 유효하지 않은 코드로 만든다. `invalidFlip(_:)` 을 고치기 위한 하나의 방법은 정사각형에 대한 특별 케이스를 `FlippedShape` 의 구현으로 옮기는 것이며, 이는 이 함수가 항상 `FlippedShape` 값을 반환하도록 한다.

```swift
struct FlippedShape<T: Shape>: Shape {
  var shape: T
  func draw() -> String {
    if shape is Square {
      return shape.draw()
    }
    let lines = shape.draw().split(separator: "\n")
    return lines.reversed().joined(separator: "\n")
  }
}
```

항상 하나의 타입을 반환해야 한다는 요구 사항은 불투명 반환 타입에 제네릭을 사용하는 것을 못하게 하지 않는다. 함수의 타입 매개 변수를 그것이 반환하는 값의 기반 타입과 통합하는 예제가 있다.

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
  return Array<T>(repeating: shape, count: count)
}
```

이 경우 반환 값의 기반 타입은 `T` 에 따라 변화한다. 어떠한 도형이 넘겨지든, `repeat(shape:count:)` 는 도형의 배열을 생성하고 반환한다. 그럼에도 불구하고, 반환 값은 항상 같은 `[T]` 의 기반 타입을 갖는다. 그러므로 이것은 불투명 반환 타입을 갖는 함수는 오직 하나의 타입의 값을 반환해야 한다는 요구 사항을 따른다.

## 불투명 타입과 프로토콜 타입의 차이

불투명 타입을 반환하는 것은 함수의 반환 타입으로 프로토콜 타입을 사용하는 것과 매우 비슷해 보인다. 하지만 이 두 가지의 반환 타입은 그것들이 타입 동등성을 보존하는지의 여부에서 차이를 보인다. 불투명 타입은 하나의 특정 타입을 참조하는데, 함수의 호출자는 어떠한 타입인지 볼 수 없다. 프로토콜 타입은 그 프로토콜을 준수하는 어떠한 타입이라도 참조할 수 있다. 일반적으로 프로토콜 타입은 값이 저장하는 기반 타입에 대해 더 많은 유연성을 제공하며, 불투명 타입은 값들의 기반 타입에 대하여 더욱 확실하게 보증할 수 있게 한다.

예를 들어 불투명 반환 타입을 사용하는 대신 프로토콜 타입의 값을 반환하는 `flip(_:)` 버전이 있다.

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
  return FlilppedShape(shape: shape)
}
```

`protoFlip(_:)` 의 해당 버전은 `flip(_:)` 과 같은 함수 구현을 가지고 있으며, 항상 같은 타입의 값을 반환한다. `flip(_:)` 과는 다르게 `protoFlip(_:)` 이 반환하는 값은 항상 같은 타입을 가질 필요가 없다. 그것은 `Shape` 프로토콜을 준수하기만 하면 된다. 바꿔 말하면, `protoFlip(_:)` 은 `flip(_:)` 이 하는 것보다 그 호출자와 훨씬 더 느슨한 API 계약을 맺는다. 이는 여러 타입을 반환할 수 있는 유연성을 보유한다.

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
  if shape is Square {
    return shape
  }
  return FlippedShape(shape: shape)
}
```

코드의 수정된 버전은 어떠한 도형이 넘겨졌는지에 따라 `Square` 또는 `FlippedShape` 의 인스턴스를 반환한다. 이 함수에 의해 반환되는 두 개의 플립된 도형은 완전히 다른 타입을 갖게 될 것이다. 이 함수의 다른 유효한 버전은 같은 도형의 여러 인스턴스를 플립할 때 다른 타입의 값을 반환할 수 있을 것이다. `protoFlip(_:)` 의 덜 특정된 반환 타입 정보는 타입 정보에 의존하는 많은 동작이 반환되는 값에 대하여 사용 가능하지 않다는 것을 의미한다. 예를 들어, 이 함수가 반환하는 결과들을 `==` 연산자를 사용하여 비교하는 것은 가능하지 않다.

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing  // 에러
```

예제의 마지막 줄에 있는 에러는 여러 개의 이유로 발생한다. 직접적인 문제는 `Shape` 가 프로토콜 요구 사항으로 `==` 연산자를 포함하지 않는다는 것이다. 이것을 추가하려 한다면, 당신이 맞닥뜨릴 다음 문제는 `==` 연산자가 왼쪽 및 오른쪽 인자의 타입을 알 필요가 있다는 것이다. 이러한 종류의 연산자는 일반적으로 프로토콜을 채택하는 구체적인 타입과 일치하는 `Self` 타입의 인자를 취하지만, 프로토콜 자체에 `Self` 요구 사항을 추가하는 것은 프로토콜을 타입으로 사용할 때 발생하는 타입 삭제*type erasure*를 허용하지 않는다.

함수에서 프로토콜 타입을 반환 타입으로 사용하는 것은 그 프로토콜을 준수하는 어떠한 타입이라도 반환할 수 있는 유연성을 제공한다. 하지만 그 유연성의 대가는 몇몇 동작이 반환 값에 대하여 가능하지 않다는 것이다. 예제는  `==` 연산자를 사용할 수 없는 방법을 보여준다. 그것은 프로토콜 타입을 사용하는 것에 의해 보존되지 않는 특정 타입 정보에 의존한다.

이 접근법의 또다른 문제는 도형 변환이 중첩되지 않는다는 것이다. 삼각형을 플립하는 것의 결과는 `Shape` 타입의 값이고, `protoFlip(_:)` 함수는 `Shape` 프로토콜을 준수하는 어떠한 타입의 인자를 취한다. 그러나 프로토콜 타입의 값은 그 프로토콜을 준수하지 않는다. `protoFlip(_:)` 이 반환하는 값은 `Shape` 를 준수하지 않는다. 이는 여러 변환을 수행하는 `protoFlip(protoFlip(smallTriangle))` 과 같은 코드는 플립된 도형이 `protoFlip(_:)` 의 유효한 인자가 아니기 때문에 유효하지 않다는 것을 의미한다.

대조적으로, 불투명 타입은 기반 타입의 동등성을 보존한다. Swift는 연관 타입을 추론할 수 있으며, 이는 프로토콜 타입이 반환 값으로 사용될 수 없는 곳에서 불투명 반환 타입을 사용할 수 있게 한다. 예를 들어 "제네릭"에 있는 `Container` 프로토콜의 한 버전이 있다.

```swift
protocol Container {
  associatedType Item
  var count: Int { get }
  subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

`Container` 프로토콜은 연관 타입을 가지고 있기 때문에 이것을 함수의 반환 타입으로 사용할 수 없다. 또한 제네릭 타입이 필요로 하는 것을 함수 구현부 바깥에서 추론하기 위한 충분한 정보가 없기 때문에 제네릭 반환 타입의 제약으로 사용할 수도 없다.

```swift
// 에러: 연관 타입을 갖는 프로토콜은 반환 타입으로 사용될 수 없다.
func makeProtocolContainer<T>(item: T) -> Container {
  return [item]
}

// 에러: C를 추론하기 위한 충분한 정보가 없다.
func makeProtocolContainer<T, C: Container>(item: T) -> C {
  return [item]
}
```

`some Container` 불투명 타입을 반환 타입으로 사용하여 바람직한 API 계약을 표현할 수 있다. 함수는 컨테이너를 반환하나 컨테이너의 타입을 지정하기 위해 decline한다.

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
  return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
type(of: twelve) // Int
```

`twelve` 의 타입은 `Int` 가 되도록 추론되며, 타입 추론이 불투명 타입과 작동한다는 사실을 나타낸다. `makeOpaqueContainer(item:)` 의 구현에서, 불투명 컨테이너의 기반 타입은 `[T]` 이다. 이 경우 `T` 는 `Int` 이므로 반환 값은 정수의 배열이며 `Item` 연관 타입은 `Int` 가 되도록 추론된다. `Container` 에 대한 서브스크립트는 `Item` 을 반환하며, 이는 `twelve` 의 타입 또한 `Int` 가 되도록 추론되었음을 의미한다.