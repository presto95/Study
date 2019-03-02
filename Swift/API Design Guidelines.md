# [API 디자인 가이드라인](https://swift.org/documentation/api-design-guidelines/)

## 기초

- 가장 중요한 목표는 **사용 시점에서의 명백함**입니다. 메소드와 프로퍼티 같은 엔티티는 오직 한 번 선언되지만 반복적으로 *사용*됩니다. 그것들을 분명하고 간결하게 사용하기 위한 API를 설계하십시오. 설계를 평가할 때, 선언을 읽는 것으로는 부족합니다. 컨텍스트에서 그것이 분명하다는 것을 확신하기 위해 항상 유즈케이스*use case*를 실험하십시오.

- **명백함은 간결함보다 더 중요합니다.** 스위프트 코드는 압축될 수 있을지라도, 최소한의 문자를 가지고 실행 가능한 가장 작은 코드를 만드는 것은 우리의 *목표가 아닙니다.* 간결한 스위프트 코드에서, 그것은 강타입 시스템의 사이드 이펙트이며 원천적으로 보일러플레이트를 줄이는 기능입니다.

- 모든 선언에 **문서화 주석을 작성하십시오.** 문서를 작성하면서 얻는 인사이트가 당신의 설계에 깊은 영향을 줄 수 있으므로, 이 작업을 미루지 마십시오.

  > 간단한 용어로 API의 기능을 묘사하는 데 어려움을 겪는다면, **아마 잘못된 API를 설계하고 있는 것일지도 모릅니다.**

  - **스위프트의 [마크다운 문법](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/)을 사용하십시오.**

  - 선언되려는 엔티티를 묘사하는 **요약으로 시작하십시오.** 종종 API는 그 선언과 요약만으로도 완전하게 이해될 수 있습니다.

    ```swift
    /// 역순으로 같은 요소를 포함하는 `self`의 "view"를 반환.
    func reversed() -> ReverseCollection
    ```

    - **요약에 집중하십시오.** 이것이 가장 중요한 부분입니다. 많은 뛰어난 문서화 주석은 훌륭한 요약만으로 구성되어 있습니다.

    - 가능하다면 마침표로 끝나는 **단일 문장 조각**을 사용하십시오. 완전한 문장을 사용하지 마십시오.

    - **함수나 메소드가 무엇을 *하는지*, 무엇을 *반환하는지*에 대해 묘사하십시오.** null 효과와 `Void` 반환은 생략하십시오.

      ```swift
      /// `self`의 시작에 `newHead` 삽입.
      mutating func prepend(_ newHead: Int)
      
      /// `self`의 요소에 뒤에 `head`를 포함하는 `List` 반환.
      func prepending(_ head: Element) -> List
      
      /// 비어 있지 않다면 `self`의 첫 번째 요소를 제거하고 반환.;
      /// 그렇지 않다면 `nil` 반환.
      mutating func popFirst() -> Element?
      ```

      > 위의 `popFirst`와 같은 드문 경우에, 요약은 세미콜론으로 분리된 여러 개의 문장 조각으로 구성됩니다.

    - **서브스크립트가 *접근하는* 것을 묘사하십시오.**

      ```swift
      /// `index`번째의 요소에 접근.
      subscript(index: Int) -> Element { get set }
      ```

    - **이니셜라이저가 *생성하는* 것을 묘사하십시오.**

      ```swift
      /// `x`를 `n`번 반복한 것을 포함하는 인스턴스 생성.
      init(count n: Int, repeatedElement x: Element)
      ```

    - 모든 다른 선언에서 **선언된 엔티티가 *무엇인지* 묘사하십시오.**

      ```swift
      /// 어떠한 위치에서의 삽입이나 삭제에도 동등한 효율을 지원하는 컬렉션.
      struct List {
        
        /// `self`의 시작 위치와 요소. `self`가 비어 있다면 `nil`.
        var first: Element?
        ...
      }
      ```

  - **선택적으로, 하나 이상의 단락과 불렛 아이템을 추가하십시오.** 단락은 빈 줄로 구분되고 완전한 문장을 사용합니다.

    ```swift
    /// 표준 출력에 `items`의 각 요소의 텍스트 표현을 출력.
    ///
    /// 각 항목 `x`에 대한 텍스트 표현은 `String(x)` 표현식으로 생성됩니다.
    ///
    /// - Parameter separator: 항목 사이에 출력될 텍스트
    /// - Parameter terminator: 끝에 출력될 텍스트
    ///
    /// - Note: 끝 개행 없이 출력하기 위해 `terminator: ""`를 넘겨주십시오.
    ///
    /// - SeeAlso: `CustomDebugStringConvertible`, `CustomStringConvertible`, 
    ///   `debugPrint`.
    public func print(_ items: Any..., 
                      separator: String = " ", 
                      terminator: String = "\n")
    ```

    - **인식 가능한 [심볼 문서화 마크업](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1) 요소를 사용**하여 적절할 때마다 요약에 정보를 추가하십시오.
    - **[심볼 커맨드 문법]()과 함께 인식 가능한 불렛 아이템을 알아두고 사용하십시오.** Xcode와 같은 잘 알려진 개발 도구는 다음의 키워드로 시작하는 불렛 아이템에 대한 특별한 처리를 제공합니다.
      - Attention / Author / Authors / Bug / Complexity / Copyright / Date / Experiment / Important / Invariant / Note / Parameter / Parameters / Postconfition / Precondition / Remark / Requires / Returns / SeeAlso / Since / Throws / ToDo / Version / Warning
      - [관련 공식 문서](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/)

## 네이밍

### 분명한 사용 촉진하기

- 그 이름이 사용된 곳에서 코드를 읽는 사람이 **모호함을 피하기 위해 필요한 모든 단어를 포함하십시오.**

  예를 들어, 컬렉션 내의 주어진 위치에 있는 요소를 제거하는 메소드를 고려합시다.

  ```swift
  // 💚
  extension List {
    public mutating func remove(at position: Index) -> Element
  }
  employees.remove(at: x)
  ```

  

  메소드 시그니쳐에서 `at`이라는 단어를 빠뜨렸다면, 이는 읽는 사람으로 하여금, `x`를 제거하려는 요소의 위치를 가리키기 위해 사용하기보다는, 메소드가 `x`와 동등한 요소를 찾고 그 요소를 제거한다는 것을 내포할 가능성이 있습니다.

  ```swift
  // ❌
  employees.remove(x)	// 분명하지 않음. x를 제거하는 것인가?
  ```

- **필요 없는 단어를 제거하십시오.** 이름에 들어 있는 모든 단어는 사용하는 위치에서 두드러진 정보를 전달해야 합니다.

  모든 단어는 의도를 명백하게 하고 의미의 모호함을 없애기 위해 필요할 수 있습니다. 그러나 읽는 사람이 이미 가진 정보와 중복된 단어는 제거되어야 합니다. 특히 *단지 타입 정보만을 반복하는 단어*를 제거하십시오.

  ```swift
  // ❌
  public mutating func removeElement(_ member: Element) -> Element?
  
  allViews.removeElement(cancelButton)
  ```

  이 경우에, `Element`라는 단어는 호출하는 위치에서 어떠한 두드러진 정보도 추가하지 않습니다. 이 API가 더 나을 것입니다.

  ```swift
  // 💚
  public mutating func remove(_ member: Element) -> Element?
  
  allViews.remove(cancelButton)	// 더 분명함
  ```

  때때로, 타입 정보를 반복하는 것은 모호함을 피하기 위해 필수적입니다. 하지만 일반적으로 그것의 타입보다는 매개변수의 *역할*을 묘사하는 단어를 사용하는 것이 더 좋습니다. 다음의 항목에서 세부 사항을 확인하십시오.

- **변수, 매개변수, 연관 타입이** 그들의 타입 제약보다는, **그들의 역할에 따라 이름 지어지도록 하십시오.**

  ```swift
  // ❌
  var string = "Hello"
  protocol ViewController {
    associatedType ViewType: View
  }
  class ProductionLine {
    func restock(from widgetFactory: WidgetFactory)
  }
  ```

  이러한 방식으로 타입 이름에 다른 목적을 갖게 하는 것은 명백함과 표현 능력을 최적화하지 못합니다. 대신 엔티티의 *역할*을 표현하는 이름을 선택하기 위해 노력하십시오.

  ```swift
  // 💚
  var greeting = "Hello"
  protocol ViewController {
    associatedType ContentView: View
  }
  class ProductionLine {
    func restock(from supplier: WidgetFactory)
  }
  ```

  연관 타입이 그 프로토콜 제약에 너무 단단히 묶여 있어 프로토콜 이름 *자체*가 역할인 경우에, 프로토콜 이름에 `Protocol`을 이어붙여서 충돌을 피하십시오.

  ```swift
  protocol Sequence {
    associatedType Iterator: IteratorProtocol
  }
  protocol IteratorProtocol { ... }
  ```

- 매개변수의 역할을 명백하게 하기 위해 **약타입 정보를 벌충하십시오.**

  매개변수 타입이 특히 `NSObject`, `Any`, `AnyObject`일 때, 또는 `Int`나 `String`과 같은 기초 타입일 때, 사용 시점에서의 타입 정보와 컨텍스트는 그 의도를 완전하게 전달하지 못할 수 있습니다. 다음의 예에서, 선언은 분명할 수 있으나 사용하는 위치에서는 모호합니다.

  ```swift
  // ❌
  func add(_ observer: NSObject, for keyPath: String)
  
  grid.add(self, for: grahpics)	// 모호함
  ```

  명백함을 회복하기 위해, **각각의 약타입 파라미터 앞에 그 역할을 묘사하는 명사를 위치시키십시오.**

  ```swift
  // 💚
  func addObserver(_ observer: NSObject, forKeyPath path: String)
  
  grid.addObserver(self, forKeyPath: graphics)	// 분명함
  ```

### 유창한 사용성을 위해 노력하기

- **메소드와 함수 이름이 사용하는 위치에서 문법적으로 영문 구절을 형성하도록 하십시오.**

  ```swift
  // 💚
  x.insert(y, at: x)	// x, z에 y를 삽입
  x.subViews(havingColor: y)	// y 색상을 갖는 x의 서브뷰들
  x.capitalizingNouns()	// x, 대문자화된 명사들
  ```

  ```swift
  // ❌
  x.insert(y, position: z)
  x.subViews(color: y)
  x.nounCapitalize()
  ```

  인자들이 호출의 핵심 의미가 아닐 때 첫 번째 인자 이후 또는 두 번째 인자 이후에 유창함이 저하되는 것은 허용됩니다.

  ```swift
  AudioUnit.instantiate(
    with: description,
    options: [.inProgress],
    completionHandler: stopProgressBar
  )
  ```

- **팩토리 메소드의 이름을 "make"로 시작하게 하십시오.** 예) `x.makeIterator()`

- **이니셜라이저와 [팩토리 메소드](https://en.wikipedia.org/wiki/Factory_method_pattern) 호출**의 첫 번째 인자는 기반 이름으로 시작하는 구절을 형성하지 않게 하십시오. 예) `x.makeWidget(cogCount: 47)`

  예를 들어, 이러한 호출들의 첫 번째 인자는 기반 이름과 동일한 구절의 일부분으로 읽히지 않습니다.

  ```swift
  // 💚
  let foreground = Color(red: 32, green: 64, blue: 128)
  let newPart = factory.makeWidget(gears: 42, spindles: 14)
  let ref = Link(target: destination)
  ```

  다음의 경우, API 작성자는 첫 번째 인자와 문법적인 연속성을 만드는 것을 시도하였습니다.

  ```swift
  // ❌
  let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
  let newPart = factory.makeWidget(havingGearCount: 42, andSpindlesCount: 14)
  let ref = Line(to: destination)
  ```

  실제로 [인자 레이블](https://swift.org/documentation/api-design-guidelines/#argument-labels)을 따르는 이 가이드라인은, 호출이 [값을 보존하는 타입 변환](https://swift.org/documentation/api-design-guidelines/#type-conversion)을 수행하지 않는다면 레이블을 가질 것이라는 것을 의미합니다.

  ```swift
  let rgbForeground = RGBColor(cmykForeground)
  ```

- **그 사이드 이펙트에 따라 함수와 메소드의 이름을 지으십시오.**

  - 사이드 이펙트가 없는 것들은 명사구로 읽혀야 합니다. 예) `x.distance(to: y)`, `i.successor()`

  - 사이드 이펙트가 있는 것들은 명령형 동사구로 읽혀야 합니다. 예) `print(x)`, `x.sort()`, `x.append()`

  - **변화하는*mutating*/변화하지 않는*nonmutating* 메소드 쌍을 일관성 있게 이름지으십시오.** 변화하는 메소드는 종종 비슷한 의미에서 변화하지 않는 변형을 가질 것입니다. 하지만 그것은 인스턴스 자체를 갱신하기보다는 새로운 값을 반환합니다.

    - 작업이 **원천적으로 동사로 묘사될 때**, 변화하는 메소드에는 명령형 동사를 사용하고, 변화하지 않는 메소드의 이름에는 "ed"나 "ing" 접미사를 붙여 이름지으십시오.

      | 변화mutating | 불변nonmutating    |
      | ------------ | ------------------ |
      | x.sort()     | z = x.sorted()     |
      | x.append(y)  | z = x.appending(y) |

      - 동사의 과거 분사 (보통 "ed"를 이어붙임) 를 사용하여 변화하지 않는 변형의 이름을 지으십시오.

        ```swift
        /// `self` 그 자체를 뒤집음.
        mutating func reverse()
        
        /// `self`의 역 복사본을 반환.
        func reversed() -> Self
        ...
        x.reverse()
        let y = x.reversed()
        ```

      - 동사가 직접적인 객체를 가져서 "ed"를 붙이는 것이 문법적으로 올바르지 않을 때는 "ing"를 붙인 동사의 현재 분사 형태로 변화하지 않는 변형의 이름을 지으십시오.

        ```swift
        /// `self`에서 모든 개행 문자를 자르기.
        mutating func stripNewLines()
        
        /// `self`에서 모든 개행 문자가 잘린 복사본 반환.
        func strippingNewLines() -> String
        ...
        s.stripNewLines()
        let oneLine = t.strippingNewLines()
        ```

    - 작업이 **원천적으로 명사로 묘사될 때**, 변화하지 않는 메소드에는 명사를 사용하고, 그것의 변화하는 변형에서는 "form" 접두사를 붙인 이름을 지으십시오.

      | 불변nonmutating    | 변화mutating        |
      | ------------------ | ------------------- |
      | x = y.union(z)     | y.formUnion(z)      |
      | j = c.successor(i) | c.formSuccessor(&i) |

- 그 사용이 불변할 때, **불리언 메소드와 프로퍼티를 사용하는 것은 리시버에 대한 주장*assertion*으로 읽혀야 합니다.** 예) `x.isEmpty`, `line1.intersects(line2)`

- ***어떤 것이 무엇인지* 묘사하는 프로토콜은 명사로 읽혀야 합니다.** 예) `Collection`

- ***능력*을 묘사하는 프로토콜은 `able`, `ible`, 또는 `ing`를 접미사로 사용하여 이름지어져야 합니다.** 예) `Equatable`, `ProgressReporting`

- 다른 **타입, 프로퍼티, 변수, 상수**의 이름은 **명사로 읽혀야 합니다.**

### 전문 용어를 잘 사용하기

> **Term of Art** : 명사 - 특정 분야나 직업 내에서 정확하고 전문화된 의미를 갖는 단어나 구절.

- 더 일반적인 단어가 또한 그 의미를 전달한다면 **유명하지 않은 용어를 사용하지 마십시오.** "skin"이 당신의 목적을 전달한다면 이를 "epidermis"라고 말하지 마십시오. 전문 용어는 필수적인 의사 소통 도구입니다. 하지만 이를 사용하지 않을 때 사라질 수 있는 결정적인 의미를 취해야 할 때만 사용되어야 합니다.

- 전문 용어를 사용하지 않는다면 **기정사실화된 의미를 고수하십시오.**

  더 일반적인 용어 대신 기술적인 용어를 사용하는 유일한 이유는 그것이 *정확하게* 어떠한 것을 표현하기 때문이며, 그렇지 않으면 모호하거나 분명하지 않을 수 있습니다. 그러므로, API는 그것의 받아들여진 의미에 따라 그 용어를 엄밀하게 사용해야 합니다.

  - **전문가를 놀라게 하지 마십시오.** 이미 그 용어에 익숙한 누군가는 우리가 그 용어에 새로운 의미를 고안해낸 것처럼 보인다면 놀라거나 아마 언짢아할 수 있을 것입니다.
  - **초심자를 혼란스럽게 하지 마십시오.** 그 용어를 배우려는 누군가는 웹 검색을 통해 그 전통적인 의미를 찾을 수 있습니다.

- **약어를 사용하지 마십시오.** 특히 비표준 약어는 효율적인 전문 용어가 됩니다. 이해하는 것은 약어가 아닌 형태로 올바르게 번역되는 것에 의존적이기 때문입니다.

  > 당신이 사용하는 어떠한 약어가 의도하는 의미는 웹 검색을 통해 쉽게 발견될 수 있어야 합니다.

- **선례를 포용하십시오.** 기존 문화를 준수하는 것을 희생하면서 완전한 초심자를 위한 용어를 최적화하지 마십시오.

  연속적인 자료 구조의 이름을 `List`와 같은 단순화된 용어를 사용하는 것보다는 `Array`를 사용하는 것이 더 좋습니다. 초심자가 `List`의 의미를 더 쉽게 이해할 것일지라도 말입니다. Array는 현대 컴퓨팅의 기초이며, 모든 프로그래머들이 Array가 무엇인지 알고 있으며, 곧 배울 것입니다. 대부분의 프로그래머에게 익숙해져 있고, 그들의 웹 검색과 질문이 보상을 받을 수 있을 용어를 사용하십시오.

  수학과 같은 특정 프로그래밍 *영역*에서, `sin(x)`와 같은 널리 사용된 용어는 `verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)`와 같은 설명적 구절보다 선호됩니다. 이러한 경우에 선례는 약어를 사용하지 말라는  가이드라인보다 더 중요합니다. 완전한 단어인 `sine`이 있을지라도, "sin(x)"는 몇 십년간 프로그래머들 사이에서, 몇 세기 동안 수학자들 사이에서 더 일반적으로 사용되었습니다.

## 컨벤션

### 일반 컨벤션

- **O(1)의 복잡도를 갖지 않는 계산 프로퍼티에 대해 문서화하십시오.** 사람들은 종종 프로퍼티 접근이 중요한 연산을 포함하고 있을 것이라는 가정을 하지 않습니다. 마음 속으로는 저장 프로퍼티를 갖고 있기 때문입니다. 그 가정이 위반될 수 있을 때 그들이 주의를 기울일 수 있도록 하십시오.

- **전역 함수보다는 메소드와 프로퍼티를 사용하십시오.** 전역 함수는 특별한 경우에만 사용되어야 합니다

  1. 명백한 `self`가 없을 때

     ```swift
     min(x, y, z)
     ```

  2. 함수에 제약 없는 제네릭이 있을 때

     ```swift
     print(x)
     ```

  3. 함수가 기정사실화된 도메인 표기법의 일부분일 때

     ```swift
     sin(x)
     ```

- **대소문자 컨벤션을 따르십시오.** 타입과 프로토콜의 이름은 대문자 캐멀 케이스를 준수합니다. 나머지는 모두 소문자 캐멀 케이스를 준수합니다.

  미국식 영어에서 흔히 모두 대문자로 표현되는 약어와 이니셜은 대소문자 컨벤션에 따라 모두 대문자이거나 소문자이어야 합니다.

  ```swift
  var utf8Bytes: [UTF8.default]
  var isRepresentableAsASCII = true
  var userSMTPServer: SecureSTMPServer
  ```

  다른 약어는 보통 단어처럼 취급되어야 합니다.

  ```swift
  var radarDetector: RadarScanner
  var enjoyScubaDiving = true
  ```

- **메소드는** 같은 기본 의미를 공유하거나 개별의 영역에서 작동할 때 **기반 이름을 공유할 수 있습니다.**

  예를 들어, 다음은 메소드들이 본질적으로 같은 일을 하기 때문에 권해집니다.

  ```swift
  // 💚
  extension Shape {
    /// `other`가 `self`의 영역 내에 있다면 `true`를 반환.
    func contains(_ other: Point) -> Bool { ... }
    
    /// `other`가 `self`의 영역 전체 내에 있다면 `true`를 반환.
    func contains(_ other: Shape) -> Bool { ... }
    
    /// `other`가 `self`의 영역 내에 있다면 `true`를 반환.
    func contains(_ other: LineSegment) -> Bool { ... }
  }
  ```

  그리고 기하학적 타입과 컬렉션은 서로 분리된 영역이므로, 다음의 것 또한 같은 프로그램에서 사용될 수 있습니다.

  ```swift
  // 💚
  extension Collection where Element: Equatable {
    /// `self`가 `sought`와 동등한 요소를 포함한다면 `true`를 반환.
    func contains(_ sought: Element) -> Bool { ... }
  }
  ```

  그러나 이러한 `index` 메소드는 다른 의미를 가지고 있으며, 다르게 이름 지어저야 합니다.

  ```swift
  // ❌
  extension Database {
    /// 데이터베이스의 검색 인덱스 다시 만들기.
    func index() { ... }
    
    /// 주어진 테이블에서 `n`번째 행 반한.
    func index(_ n: Int, inTable: TableID) -> TableRow { ... }
  }
  ```

  마지막으로, "반환 타입에 대한 오버로딩"을 하지 마십시오. 타입 추론의 경우 모호함의 원인이 될 수 있기 때문입니다.

  ```swift
  // ❌
  extension Box {
    /// 어떠한 것이든 `self`에 저장된 `Int` 반환. 그렇지 않다면 `nil` 반환.
    func value() -> Int? { ... }
    
    /// 어떠한 것이든 `self`에 저장된 `String` 반환. 그렇지 않다면 `nil` 반환.
  	func value() -> String? { ... }
  }
  ```

### 매개변수

```swift
func move(from start: Point, to end: Point)
```

- **문서화를 제공하는 매개변수 이름을 선택하십시오.** 매개변수 이름이 사용 시점의 함수나 메소드에서 보이지 않을지라도, 그것들은 중요한 설명 역할을 수행합니다.

  문서를 읽기 쉽게 하는 이름을 선택하십시오. 예를 들어, 다음의 이름들은 문서를 자연스럽게 읽을 수 있게 합니다.

  ```swift
  // 💚
  /// `predicate`를 만족하는 `self`의 요소를 포함하는 `Array` 반환.
  func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
  
  /// 요소들의 주어진 `subRange`를 `newElements`로 교체.
  mutating func replaceRange(_ subRange: Range, with newElements: [E])
  ```

  그러나 다음의 것들은 문서를 어색하고 문법에 맞지 않게 만듭니다.

  ```swift
  // ❌
  /// `includedInResult`를 만족하는 `self`의 요소를 포함하는 `Array` 반환.
  func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
  
  /// `r`이 가리키는 요소의 범위를 `with`의 컨텐츠로 교체.
  mutating func replaceRange(_ r: Range, with: [E])
  ```

- 일반적인 사용을 단순화할 때 **기본 매개변수의 이점을 취하십시오.** 하나의 일반적으로 사용되는 값을 갖는 어떠한 매개변수라도 기본을 위한 후보가 됩니다.

  기본 인자는 관련 없는 정보를 숨겨 가독성을 향상시킵니다. 예를 들어,

  ```swift
  // ❌
  let order = lastName.compare(royalFamilyName, options: [], range: nil, locale: nil)
  ```

  은 더욱 단순해질 수 있습니다.

  ```swift
  // 💚
  let order = lastName.compare(royalFamilyName)
  ```

  기본 인자는 일반적으로 메소드 패밀리를 사용할 때 선호됩니다. 그것들은 API를 이해하려는 누군가에게 인식적인 짐을 덜어주기 때문입니다.

  ```swift
  // 💚
  extension String {
    /// ...description...
    public func compare(_ other: String, 
                        options: CompareOptions = [], 
                        range: Range? = nil, 
                        locale: Locale? = nil) -> Ordering
  }
  ```

  위의 것은 단순한 것 같지는 않지만, 다음의 것보다는 훨씬 간단합니다.

  ```swift
  // ❌
  extension String {
    /// ...description 1 ...
    public func compare(_ other: String) -> Ordering
    /// ...description 2...
    public func compare(_ other: String, options: CompareOptions) -> Ordering
    /// ...description 3...
    public func compare(_ other: String, options: CompareOptions, range: Range) -> Ordering
    /// ...description 4...
    public func compare(_ other: String, 
                        options: StringCompareOptions, 
                        range: Range, 
                        locale: Locale) -> Ordering
  }
  ```

  메소드 패밀리의 모든 멤버는 개별적으로 문서화되며 사용자에 의해 이해될 필요가 있습니다. 그것들 사이를 결정하기 위해 사용자는 그것 모두를 이해할 필요가 있습니다. 그리고 예를 들어, `foo(bar: nil)`과 `foo()`는 항상 동일어가 아닌 것처럼, 이따금의 놀라운 관계는 대부분의 동일한 문서에서 사소한 차이점을 찾아내는 지루한 작업이 되게 합니다. 기본값을 갖는 하나의 메소드를 사용하여 우수한 프로그래머 경험을 제공하십시오.

- 매개변수 리스트의 **끝에 기본값을 갖는 매개변수를 위치하게 하십시오.** 기본값이 없는 매개변수는 일반적으로 메소드의 의미에서 더 중요한 것이고, 메소드를 호출하는 곳에서 그것을 사용하는 안정적인 초기 패턴을 제공합니다.

### 인자 레이블

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y)
```

- **쓸모 있게 구분될 필요가 없는 인자에 대해서 모든 레이블을 제거하십시오.** 예) `min(number1, number2)`, `zip(sequence1, sequence2)`

- **값을 보존하는 타입 변환을 수행하는 이니셜라이저에서, 첫 번째 인자 레이블을 제거하십시오.** 예) `Int64(someUInt32)`

  첫 번째 인자는 항상 변환의 소스가 되어야 합니다.

  ```swift
  extension String {
    // `x`를 주어진 진법에 대한 텍스트 표현으로 변환
    init(_ x: BigInt, radix: Int = 10)
  }
  
  text = "The value is: "
  text += String(veryLargeNumber)
  text += " and in hexadecimal, it's"
  text += String(veryLargeNumber, radix: 16)
  ```

  그럼에도 불구하고, "좁아지는" 타입 변환에서는, 좁아지는 것을 묘사하는 레이블을 사용하는 것을 추천합니다.

  ```swift
  extension UInt32 {
    /// 특정 `value`를 갖는 인스턴스 생성.
    init(_ value: Int16)
    /// `source`의 하위 32비트를 갖는 인스턴스 생성.
    init(truncating source: UInt64)
    /// `valueToApproximate`의 가장 가까운 근사치를 갖는 인스턴스 생성.
    init(saturating valueToApproximate: UInt64)
  }
  ```

  > 값을 보존하는 타입 변화는 단일성을 갖습니다. 즉, 소스 값에서 모든 차이점은 결과값의 차이의 결과를 낳습니다. 예를 들어, `Int8`에서 `Int64`로의 변환은 값을 보존합니다. 모든 별개의 `Int8` 값은 별개의 `Int64` 값으로 변환되기 때문입니다. 하지만 다른 방향으로의 변환은 값을 보존할 수 없습니다. `Int64`는 `Int8`이 표현할 수 있는 것보다 더 가능한 값을 갖기 때문입니다.
  >
  > 기존 값을 되찾는 능력은 변환이 값을 보존하는 것인가에 영향을 주지 않습니다.

- **첫 번째 인자가 전치사구의 부분을 형성할 때, 그것에 인자 레이블을 주십시오.** 그 인자 레이블은 일반적으로 전치사로 시작해야 합니다. 예) `x.removeBoxes(havingLength: 12)`

  처음 두 개의 인자가 하나의 추상의 일부분을 나타낼 때는 예외입니다.

  ```swift
  // ❌
  a.move(toX: b, y: c)
  a.fade(fromRed: b, green: c, blue: d)
  ```

  이러한 경우, 추상화를 분명하게 유지하기 위해 전치사 *다음*부터 인자 레이블을 사용하십시오.

  ```swift
  // 💚
  a.moveTo(x: b, y: c)
  a.fadeFrom(red: b, green: c, blue: d)
  ```

- **첫 번째 인자가 문법적인 구문의 일부분을 형성한다면 레이블을 제거하십시오.** 그렇지 않다면, 기반 이름 뒤에 어떠한 단어를 이어붙이십시오. 예) `x.addSubview(y)`

  이 가이드라인은 첫 번째 인자가 문법적인 구문의 일부분을 형성하지 *않는다면*, 레이블을 가져야 한다는 것을 내포합니다.

  ```swift
  // 💚
  view.dismiss(animated: false)
  let text = words.split(maxSplits: 12)
  let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
  ```

  구문이 올바른 의미를 전달하는 것은 중요합니다. 다음의 것은 문법적으로 올바를 수 있으나 잘못된 것을 표현할 수 있습니다.

  ```swift
  // ❌
  view.dismiss(false)	// dismiss하지 않는가? Bool을 dismiss하는가?
  words.split(12)	// 12라는 숫자를 split하는가?
  ```

  기본값을 갖는 인자 또한 생략될 수 있으며, 이 경우 문법적인 구문의 일부분을 형성하지 않습니다. 그러므로 그것들은 항상 레이블을 가져야 합니다.

- **다른 모든 인자에 레이블을 사용하십시오.**

## 특별 지침

- API에서 그것들이 나타나는 곳에서 **튜플 멤버에 레이블을 사용하고 클로저 매개변수에 이름을 붙이십시오.**

  이러한 이름들은 설명하는 힘을 가지고 있으며, 문서화 주석에서 참조될 수 있고, 튜플 멤버에 표현적인 접근을 제공합니다.

  ```swift
  /// 적어도 `requestedCapacity` 요소만큼 유일하게 참조된 저장소를 가지고 있음을 확신.
  ///
  /// 더 많은 저장소를 필요로 한다면, `allocate`는 할당하려는 바이트와
  /// 최대로 정렬된 숫자와 동등한 `byteCount`와 함께 호출된다.
  ///
  /// - Returns:
  ///   - reallocated: 새로운 메모리 블록이 할당되었다면 `true`.
  ///   - capacityChanged: `capacity`가 갱신되었다면 `true`.
  mutating func ensureUniqueStorage(
    minimumCapacity requestedCapacity: Int, 
    allocate: (_ byteCount: Int) -> UnsafePointer<Void>
  ) -> (reallocated: Bool, capacityChanged: Bool)
  ```

  클로저 매개변수로 사용된 이름들은 전역 함수를 위한 매개변수 이름처럼 선택되어야 합니다. 호출 지점에서 보이는 클로저 인자를 위한 레이블은 지원되지 않습니다.

- 오버로드 세트에서 모호함을 피하기 위해 **제약 없는 다형성에 특별한 주의를 기울이십시오.** 예) `Any`, `AnyObject`, 제약 없는 제네릭 매개변수

  예를 들어 다음과 같은 오버로드 세트를 고려합시다.

  ```swift
  // ❌
  struct Array {
    /// `self.endIndex`에 `newElement` 삽입.
    public mutating func append(_ newElement: Element)
    
    /// 순서대로, `self.endIndex`에 `newElements`의 컨텐츠 삽입.
    public mutating func append(_ newElements: S) where S.Generator.Element == Element
  }
  ```

  이러한 메소드는 의미론적인 패밀리를 형성하며, 인자의 타입은 뚜렷하게 구별되기 위해 첫 번째에 보입니다. 그러나 `Element`가 `Any`일 때, 하나의 요소는 시퀀스 요소와 같은 타입을 가질 수 있습니다.

  ```swift
  // ❌
  var values: [Any] = [1, "a"]
  values.append([2, 3, 4])	// [1, "a", [2, 3, 4]] 인가, [1, "a", 2, 3, 4] 인가?
  ```

  모호함을 없애기 위해, 두 번째 오버로드를 더 명백하게 이름 지으십시오.

  ```swift
  // 💚
  struct Array {
    /// `self.endIndex`에 `newElement` 삽입.
    public mutating func append(_ newElement: Element)
    
    /// 순서대로, `self.endIndex`에 `newElements`의 컨텐츠 삽입.
    public mutating func append(contentsOf newElements: S) where S.Generator.Element == Element
  }
  ```

  새로운 이름이 문서화 주석과 얼마나 더 잘 일치하는지에 주목하십시오. 이 경우, 문서화 주석을 작성하는 행동은 실제로 API 작성자의 주의에 이슈를 가져다 주었습니다.