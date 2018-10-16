# [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)

## 기초

- **사용 시점에서의 명백함**은 가장 중요한 목표입니다. 메소드나 프로퍼티 같은 엔티티는 오직 한 번만 선언되지만 반복적으로 *사용됩니다*. 이것들을 분명하고 간결하게 사용할 수 있도록 API를 디자인하십시오. 디자인을 평가할 때, 선언을 읽는 것은 충분하지 않습니다. 그것이 문맥 안에서 분명한지 확실히 하기 위해 항상 실제 사용 경우를 시험하십시오.

- **뚜렷함은 간결함보다 훨씬 더 중요합니다.** 스위프트 코드가 간결해질 수 있더라도, 가장 적은 문자들로 가장 작은 실행 가능한 코드를 작성할 수 있는 것은 우리의 목표가 아닙니다. 스위프트 코드에서의 간결함은 강타입 시스템에서의 부작용이며, 자연스럽게 상용문을 줄이는 기능입니다.

- 모든 선언에 **문서 주석을 작성하십시오.** 문서화하면서 얻는 인사이트는 당신의 디자인에 해박한 영향을 줄 수 있으므로, 회피하려 하지 마십시오.

  > 당신이 작성한 API의 기능을 간단한 용어로 묘사하는 데 어려움을 겪는다면, 잘못된 API를 디자인한 것일 수 있습니다.
  - **[스위프트에 특화된 마크다운](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/)을 사용하십시오.**

  - 선언된 엔티티를 묘사하는 **요약으로 시작하십시오.** 종종 API는 그 선언과 요약으로부터 완벽하게 이해될 수 있습니다.

    > **Returns** a "view" of `self` containing the same elements in reverse order.

    - **요약에 집중하십시오.** 이것이 가장 중요한 부분입니다. 많은 훌륭한 문서화 주석들은 훌륭한 주석 이상 말고는 어떠한 것도 포함하고 있지 않습니다.

    - 가능하다면 마침표로 맺어지는 **간단한 한 문장의 단편을 사용하십시오**. 완전한 문장을 사용하지 마십시오.

      - 합니다. (x) 함. (o)

    - **함수나 메소드가 무슨 일을 하고 무엇을 반환하는지 묘사하십시오.** null 효과 및 `Void` 반환은 생략하십시오.

      > **Inserts** `newHead` at the beginning of `self`.
      >
      > **Returns** a `List` containning `head` followed by the elemtns of `self`.
      >
      > **Removes and returns** the first element of `self` if non-empty; returns `nil` otherwise.
      >
      > 드문 경우에, 요약은 세미콜론으로 분리된 여러 개의 문장 단편들로 구성될 수 있습니다.

    - **서브스크립트가 접근하는 것을 묘사하십시오.**

      > **Accesses** the `index`th element.

    - **생성자가 생성하는 것을 묘사하십시오.**

      > **Creates** an instance containing `n` repititions of `x`.

    - 모든 다른 선언에서, **선언된 엔티티가 무엇을 하는지 묘사하십시오.**

      > **A collection that** supports equally efficient insertion/removal at any position.
      >
      > **The element at the beginning** of `self`, or `nil` if self is empty.

  - **선택 사항으로**, 하나 이상의 단락과 글머리 기호 항목을 사용하여 **문장을 이어나가십시오**. 단락은 빈 줄로 구분되고 완전한 문장을 사용합니다.

    > 요약 / 빈 줄 / 추가적인 설명(완전한 문장으로)
    - 적절할 때마다 요약을 넘어선 정보를 추가하기 위해 **[인식된 기호 문서화 마크업 요소](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)를 사용하십시오.**
      - description / parameters / throws / returns 등
    - **[기호 명령 구문](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)으로 인식된 글머리 기호 항목을 알고 사용하십시오.** Xcode와 같은 인기 있는 개발 도구는 다음과 같은 키워드로 시작하는 글머리 기호들에 특별한 처리를 합니다.
      - Attention / Author / Authors / Bug / Complexity / Copyright / Date / Experiment / Important / Invariant / Note / Parameter / Parameters / Postcondition / Precondition / Remark / Requires / Returns / SeeAlso / Since / Throws / ToDo / Version / Warning

## 네이밍

### 명백한 사용 촉진하기

- 코드를 읽는 사람이 그 이름이 사용된 곳에서 **모호함을 피하기 위해 필요한 모든 단어들을 포함하십시오.**

  - 예를 들어, 컬렉션 내에서 주어진 위치에 있는 요소를 삭제하는 메소드를 생각해 봅시다.

    > `public mutating func remove(at position: Index) -> Element`

  - 메소드 시그니쳐에서 `at` 을 빠뜨린다면, 읽는 사람들은 그 메소드가 `x` 를 삭제할 요소의 위치를 가리키는 것으로 사용한다고 이해하기보다는, `x` 와 같은 요소를 찾아서 삭제한다고 이해할 가능성이 있습니다.

    > `employees.remove(x)` : x를 삭제하려고 하는 것인가?

- **필요 없는 단어들을 빼십시오.** 이름에 사용된 모든 단어는 사용 사이트에서 중요한 정보를 전달해야 합니다.

  - 더 많은 단어들은 의도를 명백하게 하거나 의미를 모호하지 않게 하는 데 필요할 수 있으나, 읽는 사람이 이미 알아챈 정보에 대해 중복된 것들은 생략되어야 합니다. 특히, *단순히* 타입 정보를 반복하는 단어는 생략하십시오.

    > 나쁜 예 : `public mutating func removeElement(_ member: Element) -> Element?`

  - 이 경우에, `Element` 라는 단어는 호출부에서 두드러진 부분을 추가하지 않습니다. 다음의 API가 더 나아 보입니다.

    > 좋은 예 : `public mutating func remove(_ member: Element) -> Element?`

  - 때때로, 타입 정보를 반복하는 것은 모호함을 피하기 위해 필수적이나, 일반적으로 그것의 타입 대신에 매개변수의 *역할* 을 묘사하는 단어를 사용하는 것이 더 좋습니다. 다음 내용에서 세부적인 부분을 확인하십시오.

- 타입 제약사항보다는, **역할에 따라서 변수, 매개변수, 연관 타입의 이름을 지으십시오.**

  > 나쁜 예 : `var string = "Hello"` / `associatedType ViewType: View` 

  - 위와 같은 방식으로 타입 이름을 다시 모으는 것은 뚜렷함과 표현력을 최적화하지 못합니다. 대신에, 엔티티의 *역할* 을 표현하는 이름을 선택하기 위해 노력하십시오.

    > 좋은 예 : `var greeting = "Hello"` / `associatedType ContentView: View`

  - 만약 연관 타입이 프로토콜 제약사항에 매우 단단하게 묶여있어서 프로토콜의 이름 자체가 그 역할이라면, 연관 타입의 이름에 `Type` 을 붙여서 충돌을 피하십시오.

    > `associatedType IteratorType: Iterator`

- 매개변수의 역할을 명확히 하기 위해 **약한 타입 정보를 보상합니다.**

  - 특히 매개변수의 타입이 `NSObject`, `Any`, `AnyObject` 이거나, `Int` 나 `String` 같은 기본 타입일 때, 사용 시점에서 타입 정보와 문맥은 완벽하게 의도를 전달하지 못할 수 있습니다. 아래의 예제에서, 선언은 명백하나, 사용 사이트가 모호합니다.

    > ```swift
    > //나쁜 예
    > func add(_ observer: NSObject, for keyPath: String)
    > grid.add(self, for: graphics)
    > ```

  - 명백함을 되찾기 위해, **각각의 약한 타입을 가진 매개변수의 앞에 그 역할을 묘사하는 명사를 붙이십시오.**

    > ```swift
    > //좋은 예
    > func addObserver(_ observer: NSObject, forKeyPath: String)
    > grid.addObserver(self, forKeyPath: graphics)
    > ```

### 유창한 사용을 위해 노력하기

- **사용 사이트가 문법적인 영어 구*phrase*를 형성하게끔 메소드와 함수의 이름을 짓도록 하십시오.**

  > ```swift
  > //좋은 예
  > x.insert(y, at: z) //x, z에 y를 삽입
  > x.subViews(havingColor: y) //x, y 색을 갖는 서브뷰
  > //나쁜 예
  > x.insert(y, position: z)
  > x.subViews(color: y)
  > ```

  - 첫 번째 또는 두 번째 매개변수 후로 유창성이 저하되는 것은 이 매개변수들이 호출의 의미의 중심에 있지 않을 때 받아들여질 수 있습니다.

  > `AudioUnit.instantiate(with: description, options: [.inProcess], completionHandler: stopProgressBar)`

- **팩토리 메소드의 이름의 시작을 make로 하십시오.** ex) `x.makeIterator()`

- **생성자와 [팩토리 메소드](https://en.wikipedia.org/wiki/Factory_method_pattern) 호출**의 첫 번째 인자는 기본 이름으로 시작하는 구문을 형성하지 않아야 합니다. ex) `x.makeWidget(cogCount: 47)`

  - 예를 들어서, 아래의 호출들의 첫 번째 인자들은 기본 이름과 동일한 구문의 일부로 읽히지 않습니다.

    > ```swift
    > //좋은 예
    > let foreground = Color(red: 32, green: 64, blue: 128)
    > let newPart = factory.makeWidget(gears: 42, spindles: 14)
    > ```

  - 이어서, API 작성자는 첫 번째 인자로 문법적 연속성을 만드려고 했습니다.

    > ```swift
    > //나쁜 예
    > let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
    > let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    > ```

  - 실제로, [전달인자 레이블](https://swift.org/documentation/api-design-guidelines/#argument-labels)에 관한 것과 함께 이 가이드라인은 [값을 보존하는 타입 변환](https://swift.org/documentation/api-design-guidelines/#type-conversion)을 수행하는 호출이 아니라면 첫 번째 인자는 레이블을 가질 것임을 의미합니다.

    > ```swfit
    > let rgbForeground = RGBColor(cmykForeground)
    > ```


- **사이드 이펙트에 따라 함수와 메소드의 이름을 지으십시오.**

  - 사이드 이펙트가 없는 것들은 명사 구문으로 읽혀야 합니다. ex) `x.distance(to: y)` / `i.successor()`

  - 사이드 이펙트가 있는 것들은 명령형 동사 구문으로 읽혀야 합니다. ex) `print(x)` / `x.sort()` / `x.append(y)`

  - **mutating 메소드와 nonmutating 메소드 쌍의 이름을 일관되게 지으십시오.** mutating 메소드는 종종 비슷한 의미론을 갖는 nomutating 메소드를 갖지만, nonmutating 메소드는 현재 위치에서 인스턴스를 갱신하기보다는 새로운 값을 반환합니다.

    - 동작이 **순수하게 동사로 묘사**될 때, mutating 메소드에는 동사의 명령형을 사용하고, 그것에 상응하는 nonmutating 메소드에는 `ed` 나 `ing` 같은 접미사를 붙여 이름 지으십시오.

      | Mutating    | Nonmutating        |
      | ----------- | ------------------ |
      | x.sort()    | z = x.sorted()     |
      | x.append(y) | z = x.appending(y) |

      - 동사의 과거분사형을 사용하여 nonmutating 메소드의 이름을 짓도록 하십시오. (보통 `ed` 를 붙임)

      - 동사가 직접적인 객체라서 `ed` 를 붙이는 것이 문법적으로 맞지 않을 때, `ing` 를 붙여 동사의 현재분사형을 사용하여 nonmutating 메소드의 이름을 지으십시오.

      - 동작이 **순수하게 명사로 묘사**될 때, nonmutating 메소드의 이름에 명사를 사용하고 이에 상응하는 mutating 메소드에는 접두사로 `form` 을 붙여 이름 지으십시오.

        | Mutating            | Nonmutating        |
        | ------------------- | ------------------ |
        | y.formUnion(z)      | x = y.union(z)     |
        | c.formSuccessor(&i) | j = c.successor(i) |

- 그 사용이 nonmutating 할 때, **불리언 메소드와 프로퍼티들은 리시버에 대한 주장*assertions about the receiver* 으로 읽혀져야 합니다.** Ex) `x.isEmpty` / `line1.intersects(line2)`
- **어떤 것이 무엇인지**를 묘사하는 프로토콜은 명사로 읽혀져야 합니다. ex) `Collection`
- **능력**을 묘사하는 프로토콜은 `able`, `ible` 또는 `ing` 같은 접미사가 붙은 이름을 사용해야 합니다. ex) `Equatable` / `ProgressReporting`
- 다른 **타입, 프로퍼티, 변수 그리고 상수의 이름은 명사로 읽혀져야 합니다.**

### 용어를 잘 사용하기

**Term of Art** : 명사. 특정 필드나 직업 내에서 정확한, 전문화된 의미를 갖는 단어나 구절. *이하 전문용어.*

- 보다 일반적인 단어가 또한 의미를 잘 전달하는 경우에는 **모호한 용어는 피하십시오.** `skin` 이 당신의 목적을 잘 제공한다면 `epidermis` 라고 말하지 마십시오. 전문용어는 필수적인 의사 소통 도구이지만, 그것을 사용하지 않았을 때 잃어버릴 수 있는 중요한 의미를 포착할 때에만 사용해야 합니다.
- 전문용어를 사용한다면 **확립된 의미에 충실하십시오.**
  - 보다 일반적인 단어 대신 기술적인 용어를 사용하는 유일한 이유는 다른 단어를 사용했을 때 모호하고 명백하지 않을 어떤 것을 정확하게 표현하기 위함입니다. 그러므로, API는 허용된 의미에 따라 용어를 엄격히 사용해야 합니다.
    - **전문가를 놀라게 하지 마십시오.** 이미 그 용어에 친숙해져 있는 사람들은 우리가 그것에 새로운 의미를 고안해 넣은 것 같아 보인다면 놀라거나 언짢아할 수 있을 것입니다.
    - **초심자를 혼란스럽게 하지 마십시오.** 그 용어를 배우려고 하는 사람들은 인터넷 검색을 하고 그것의 전통적인 의미를 찾을 수 있습니다.
- **축약형을 사용하는 것을 피하십시오.** 특히 비표준 축약형은 효과적인 전문용어가 됩니다. 그것을 이해하는 것은 축약형이 아닌 형태로 올바르게 변환하는 데 달려있기 때문입니다.
  - 당신이 사용하는 어떠한 축약형이 의도하는 의미는 인터넷 검색을 통해 쉽게 찾을 수 있어야 합니다.
- **선례를 포용하십시오.** 기존 문화에 대한 적합성을 희생하면서 용어를 초보자를 위해 최적화하지 마십시오.
  - 연속적인 자료구조에 `List` 같은 쉬운 용어를 사용하는 것보다 `Array` 라과 이름짓는 것이 나은데, 초심자가 `List` 의 의미를 더 쉽게 이해할 수 있을지라도 그렇습니다. 배열은 현대 컴퓨팅에서 기본적인 것이고, 모든 프로그래머가 알고 있거나 곧 배열이 무엇인지 배울 것입니다. 대부분의 프로그래머에게 친숙하고, 인터넷 검색과 질문에서 답을 얻기 쉬운 용어를 사용하십시오.
  - 수학과 같은 특정한 프로그래밍 도메인에서, `sin(x)` 와 같은 널리 선행된 용어는 `verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)` 같은 설명 구절보다 선호됩니다. 이 경우에, `sine` 이라는 완전한 단어가 있을지라도 `sin(x)` 는 수십 년간 프로그래머들 사이에서, 몇 세기동안 수학자들 사이에서 흔하게 사용되었으므로, 선례는 축약형을 피하라는 가이드라인의 중요도보다 높습니다. 

## 컨벤션

### 일반적인 컨벤션

- **O(1)의 시간복잡도를 갖지 않는 계산 프로퍼티에 대해서 문서화하십시오.** 사람들은 그 프로퍼티에 접근하는 것이 중요한 연산을 포함하지 않을 것이라고 가정할 것인데, 그것들은 정신적 모델*mental model*로 저장 프로퍼티를 가지고 있기 때문입니다. 가정이 위반될 때 경고하는 것을 확실히 해야 합니다.

- **자유 함수*free function*보다는 메소드와 프로퍼티를 선호하십시오.** 자유 함수는 특별한 경우에서만 사용됩니다.

  - 명백한 `self` 가 없을 때

    > `min(x, y, z)`

  - 함수가 제약 없는 제네릭일 때

    > `print(x)`

  - 함수 구문이 확립된 영역의 표기법의 일부일 때

    > `sin(x)`

- **대소문자 컨벤션을 따르십시오.** 타입과 프로토콜의 이름은 대문자 카멜케이스를 사용합니다. 다른 모든 것들은 소문자 카멜케이스를 사용합니다.

  - 흔히 미국식 영어에서 모두 대문자로 되어 있는 [약어와 단어의 앞글자를 따서 만들어진 단어](https://en.wikipedia.org/wiki/Acronym)는 대소문자 컨벤션에 의해 균일하게 대문자 또는 소문자로 이루어져야 합니다.

    > `var utf8Bytes: [UTF8.CodeUnit]` : UTF8
    >
    > `var isRepresentableAsASCII = true` : ASCII
    >
    > `var userSMTPServer: SecureSMTPServer` : SMTP

  - 다른 약어들은 일반적인 단어처럼 취급되어야 합니다.

    > `var radarDetector: RadarScanner` : Radar
    >
    > `var enjoysScubaDiving = true` : Scuba

- 같은 기본 의미를 공유하거나 특정 영역에서 동작할 때 **메소드들은 기본 이름을 공유할 수 있습니다.**

  - 아래의 예시는 권장되는데, 메소드들이 본질적으로 같은 것을 하기 때문입니다.

    ```swift
    //좋은 예시
    extension Shape {
        func contains(_ other: Point) -> Bool {...}
    	func contains(_ other: Shape) -> Bool {...}
    	func contains(_ other: LineSegment) -> Bool {...}
    }
    ```

  - 그리고 기하학적 타입과 컬렉션이 분리된 영역에 있기 때문에, 같은 프로그램에서도 또한 괜찮습니다.

    ```swift
    //좋은 예시
    extension Collection where Element: Equatable {
        func contains(_ sought: Element) -> Bool {...}
    }
    ```

  - 하지만 아래의 `index` 메소드들은 다른 의미를 가지고 있고, 다르게 이름 지어져야 합니다.

    ```swift
    //나쁜 예시
    extension Database {
        func index() {...}
        func index(_ n: Int, inTable: TableID) -> TableRow {...}
    }
    ```

  - 마지막으로, 타입 추론에 모호함을 초래하기 때문에 반환 형식을 오버로딩하는 것을 피하십시오.

    ```swift
    //나쁜 예시
    extension Box {
        func value() -> Int? {...}
     	func value() -> String? {...}   
    }
    ```

### 매개변수

> `func move(from start: Point, to end: Point)`

- **문서화된 것을 제공할 수 있는 매개변수 이름을 선택하십시오.** 매개변수 이름이 함수나 메소드의 사용 시점에 보이지 않는다 하더라도, 그것들은 중요한 설명의 역할을 수행합니다.

  - 문서화된 것을 읽기 쉽게 하는 이름을 선택하십시오. 예를 들어, 이러한 이름들은 문서를 자연스럽게 읽도록 합니다.

    ```swift
    /// Return an `Array` containing the elements of `self` that satisfy `predicate`.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
    ```

  - 그러나, 아래와 같은 것들은 문서를 어색하고 문법적으로 맞지 않게 합니다.

    ```swift
    /// Return an `Array` containing the elements of `self` that satisfy `includedInResult`.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
    ```

- 일반적인 사용을 단순화할 때 **기본 매개변수의 이점을 취하십시오.** 하나의 공통적으로 사용되는 값을 취하는 어떤 매개변수는 기본값을 갖는 매개변수의 후보가 됩니다.

  - 기본 인자는 관련 없는 정보를 숨김으로써 가독성을 향상시킵니다.

    `let order = lastName.compare(royalFamilyName, options: [], range: nil, locale: nil)`

  - 위의 코드는 다음처럼 훨씬 간단해질 수 있습니다.

    `let order = lastName.compare(royalFamilyName)`

  - 기본 인자는 일반적으로 메소드 패밀리의 사용에서 선호되는데, API를 이해하려는 사람들에게 인지적 부담을 적게 주기 때문입니다.

    ```swift
    extension String {
        public func compare(_ other: String, options: CompareOptions = [], range: Range? = nil, locale: Locale? = nil) -> Ordering
    }
    ```

  - 위의 코드는 간단해보이지 않을 수 있지만, 아래보다는 훨씬 간단합니다.

    ```swift
    extension String {
        public func compare(_ other: String) -> Ordering
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        public func compare(_ other: String, options: CompareOptions, range: Range) -> Ordering
        public func compare(_ other: String, options: CompareOptions, range: Range, locale: Locale) -> Ordering
    }
    ```

  - 메소드 패밀리의 모든 멤버들은 사용자들에 의해 분리되어 문서화되고 이해될 필요가 있습니다. 그것들 사이를 결정하기 위해, 사용자는 그것들 모두를 이해할 필요가 있고, 때때로 그 관계에 대해 놀랍니다. 예를 들어, `foo(bar: nil)` 과 `foo()` 는 항상 동의어인 것은 아닙니다.  ,,,,,,,,,

- **기본값이 있는 매개변수를 매개변수 리스트의 끝에 위치시키도록 하십시오.** 기본값이 없는 매개변수는 보통 메소드의 의미에 있어서 더 본질적이고, 메소드가 호출되는 곳에서 안정된 생성 패턴을 제공합니다.

### 전달인자 레이블

> `func move(from start: Point, to end: Point)` / `x.move(from: x, to: y)`

- **인자가 유용하게 구별되지 않는다면 모든 레이블을 생략하십시오.** ex)`min(number1, number2)` / `zip(sequence1, sequence2)`

- **값 보존 타입 변환을 수행하는 생성자에서는 첫 번째 전달인자 레이블을 생략하십시오.** ex) `Int64(someUInt32)`

  - 첫 번째 인자는 항상 변환의 근원이 되어야 합니다.

    ```swift
    extension String {
        init(_ x: BigInt, radix: Int = 10)	// x를 10진수 숫자로 변환
    }
    ```

  - 그럼에도 불구하고, "유형을 좁히는" 형식 전환에서는 유형을 좁히는 레이블을 묘사하는 것을 추천합니다.

    ```swift
    extension UInt32 {
        init(truncating source: UInt64)	//source의 아래 32비트를 갖는 인스턴스 생성
    }
    ```

  > 값을 보존하는 타입 변환은 단일성*monomorphism*입니다. 즉, 근원 값의 모든 차이는 결과 값의 차이를 초래합니다. 예를 들어, Int8에서 Int64로의 변환은 값을 보존하는데, 모든 고유 Int8 값이 고유 Int64 값으로 변환되기 때문입니다. 그러나, 다른 방향으로의 변환은 값 보존을 할 수 없습니다. Int64는 Int8에서 표현할 수 있는 값보다 더 많은 값을 가질 수 있기 때문입니다.
  >
  > 즉, 기존 값을 되찾는 능력은 변환이 값 보존인지 여부에 영향을 주지 않습니다.

- **첫 번째 인자가 전치사구의 일부를 형성할 때, 그것을 전달인자 레이블로 지정하십시오.** 전달인자 레이블은 일반적으로 전치사에서 시작해야 합니다. ex) `x.removeBoxes(havingLength: 12)`

  - 첫 두 개의 인자가 단일 추상화의 일부를 나타낼 때 예외가 발생합니다.

    ```swift
    //나쁜 예
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

  - 이러한 경우, 전달인자 레이블을 전치사 *다음에* 붙이십시오. 추상화를 명백하게 하기 위해서입니다.

    ```swift
    //좋은 예
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

- **그렇지 않다면, 첫 번째 인수가 문법적 구문의 일부를 형성한다면, 이전에 위치하는 단어를 기본 이름에 추가하여 레이블을 생략합니다.** ex) `x.addSubview(y)`

  - 이 가이드라인은 첫 번째 인자가 문법적 구문의 일부를 형성하지 *않는다면*, 레이블을 가져야 한다는 것을 암시합니다.

    ```swift
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    ```

  - 구문이 올바른 의미를 전달하는 것은 중요합니다. 아래의 코드는 문법적으로 올바르지만 잘못된 것을 표현할 지도 모릅니다.

    ```swift
    view.dismiss(false)	//dismiss하지 않는 것인가? Bool을 dismiss하는가?
    words.split(12)		//12라는 숫자를 split하는 것인가?
    ```

  - 또한 기본값을 가진 인자들도 생략될 수 있으며, 이러한 경우 문법적 구문의 일부를 형성하지 않고, 그러므로 항상 레이블을 가져야 함을 알아두십시오.

- **모든 다른 인자에 레이블을 붙이십시오.**

## 특별 지침

- **튜플 멤버에 레이블을 붙이고 클로저 매개변수에 이름을 붙이십시오.**

  - 이러한 이름들은 설명력을 가지며, 문서 주석으로부터 참조될 수 있고, 튜플 멤버들에 대한 표현적인 접근을 제공합니다.

    ```swift
    mutating func ensureUniqueStorage(minimumCapacity requestedCapacity: Int, allocate: (_ byteCount: Int) -> UnsafePointer<Void>) -> (reallocated: Bool, capacityChanged: Bool)
    ```

  - 클로저 매개변수에 사용되는 이름들은 최상위 수준 함수의 매개변수 이름과 같이 선택되어야 합니다. 호출 사이트에서 나타나는 클로저 인자를 위한 레이블은 지원되지 않습니다.

- 오버로딩 집합들에서 모호함을 피하기 위해 **제약 없는 다형성에 더 주의를 기울이십시오.** ex) `Any` / `AnyObject` / 제약 없는 제네릭 매개변수들

  - 예를 들어, 다음과 같은 오버로딩 집합을 고려하십시오.

    ```swift
    struct Array {
        public mutating func append(_ newElement: Element)
        public mutating func append(_ newElement: S) where S.Generator.Element == Element
    }
    ```

  - 이 메소드들은 의미론적으로 가족을 형성하고, 인수 유형은 처음에는 날카롭게 구분되는 것처럼 보입니다. 그러나, `Element` 가 `Any` 일 때, 단일 요소는 요소들의 시퀀스처럼 같은 타입을 가질 수 있습니다.

  - 모호함을 없애기 위해, 두 번째 오버로딩을 더 명확하게 이름 지으십시오.

    ```swift
    struct Array {
        public mutating func append(_ newElement: Element)
        public mutating func append(contentsOf newElement: S) where S.Generator.Element == Element
    }
    ```

  - 새로운 이름이 문서 주석에 어떻게 더 잘 들어맞는지 확인하십시오. 이 경우에, 문서화 주석을 작성하는 행위는 실제로 API 저자의 주의에 이슈를 불러일으킬 것입니다.