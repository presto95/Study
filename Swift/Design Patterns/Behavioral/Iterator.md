# Iterator

### 반복자 패턴

> 반복자 패턴은 기반 구조를 이해할 필요 없이 집계 객체의 항목 모음을 탐색하기 위한 표준 인터페이스를 제공하기 위해 사용된다.

내부를 알 필요 없이 구성된 자료 구조를 탐색할 수 있게 한다.

컨테이너로부터 알고리즘을 분리시킨다.

컬렉션의 인덱스 접근에 의한 예외, 요소의 개수 등에 신경쓰지 않아도 되게 된다.

### 코드

Swift가 제공하는 IteratorProtocol, Sequence 프로토콜을 활용할 수 있다.

- IteratorProtocol
  - 한 번에 하나씩 시퀀스의 값을 제공하는 타입
  - Sequence 프로토콜과 밀접한 관계가 있음
    - Sequence 프로토콜의 Iterator 연관 타입은 IteratorProtocol을 채택하는 타입이어야 한다.
- Sequence
  - 요소에 순차적이고 반복적인 접근을 제공하는 타입

IteratorProtocol 프로토콜은 iterator에 의해 탐색되는 요소의 타입인 `Element` 를 연관 타입으로 갖고, 다음 요소를 탐색하는 `next()` 메소드를 요구한다.

Swift에서 `for-in` 반복문을 사용하려면 Sequence 프로토콜을 채택해야 한다.

결과적으로 `Array<Novella>` 가 아닌 반복자 패턴을 활용하여 `Novellas` 타입을 순회할 수 있도록 코드를 작성한다.

```swift
struct Novella {
  let name: String
}

struct Novellas {
  let novellas: [Novella]
}

// 반복자 패턴을 구현하기 위한 타입
struct NovellasIterator: IteratorProtocol {
  private var current = 0
  private let novellas: [Novella]
  
  init(novellas: [Novella]) {
    self.novellas = novellas
  }
  
  mutating func next() -> Novella? {
    defer {
      current += 1
    }
    return novellas.count > current ? novellas[current] : nil
  }
}

extension Novellas: Sequence {
  func makeIterator() -> NovellasIterator {
    return NovellasIterator(novellas: novellas)
  }
}

/* 사용 */

let greatNovellas = Novellas(novellas: [Novella(name: "The Mist")])

for novella in greatNovellas {
  print("I've read: \(novella)")
}
```