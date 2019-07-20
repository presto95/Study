# Composite

### 컴포지트 패턴

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