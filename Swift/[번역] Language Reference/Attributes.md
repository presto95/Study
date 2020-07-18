# 애트리뷰트

Swift에는 두 가지 종류의 애트리뷰트가 있다-선언에 적용되는 것들과, 타입에 적용되는 것들. 애트리뷰트는 선언이나 타입에 대한 추가 정보를 제공한다. 예를 들어, 함수 선언에서 `discardableResult` 애트리뷰트는 함수가 값을 반환할지라도, 반환 값이 사용되지 않을 때 컴파일러가 경고를 내지 않는다는 것을 가리킨다.

`@` 기호 뒤에 애트리뷰트의 이름과 해당 애트리뷰트가 받는 인자들을 작성하여 애트리뷰트를 지정한다.

```swift
@{attribute name}
@{attribute name}({attribute arguments})
```

몇몇 선언 애트리뷰트들은 애트리뷰트에 대해 더 많은 정보를 지정하고 어떻게 특정 선언에 적용되는지를 지정하는 인자를 받는다. 이러한 *attribute arguments*는 소괄호로 둘러싸여 있고, 그것들의 형식은 이들이  속한 애트리뷰트에 의해 정의된다.

## 선언 애트리뷰트

선언에만 선언 애트리뷰트를 적용할 수 있다.

### available

이 애트리뷰트를 적용하여 선언의 특정 Swift 언어 버전이나 특정 플랫폼 및 운영체제 버전과 관련된 라이프 사이클을 가리킨다.

`available` 애트리뷰트는 항상 두 개 이상의 콤마로 구분된 애트리뷰트 인자들의 리스트와 함께 나타난다. 이러한 매개변수들은 다음의 플랫폼 또는 언어 이름 중 하나로 시작한다.

- `iOS`
- `iOSApplicationExtension`
- `macOS`
- `macOSApplicationExtension`
- `macCatalyst`
- `macCatalystApplicationExtension`
- `watchOS`
- `watchOSApplicationExtension`
- `tvOS`
- `tvOSApplicationExtension`
- `swift`

선언의 사용 가능성을 위에 나열한 모든 플랫폼 이름들로 가리키기 위해 애스터리스크 (`*`) 또한 사용할 수 있다. Swift 버전 번호를 사용하는 사용 가능성을 지정하는 `available` 애트리뷰트는 애스터리스크를 사용할 수 없다.

남은 인자들은 어떠한 순서로든 나타날 수 있고, 중요한 마일스톤을 포함하여 선언의 라이프 사이클에 대한 추가 정보를 지정한다.

- `unavailable` 인자는 선언이 특정 플랫폼에서 사용 가능하지 않다는 것을 가리킨다. 이 인자는 Swift 버전 사용 가능성을 지정할 때 사용할 수 없다.

- `introduced` 인자는 선언이 도입된 특정 플랫폼이나 언어의 최초 버전을 가리킨다. 다음의 형식을 갖는다.

  ```swift
  introduced: {version number}
  ```

  *version number*는 구두점으로 구분된 한 개에서 세 개 까지의 양의 정수로 구성된다.

- `deprecated` 인자는 선언이 더 이상 사용되지 않는 특정 플랫폼이나 언어의 최초 버전을 가리킨다. 다음의 형식을 갖는다.

  ```swift
  deprecated: {version number}
  ```

  선택적인 *version number*는 구두점으로 구분된 한 개에서 세 개 까지의 양의 정수로 구성된다. 버전 번호를 생략하여, 언제 deprecation이 발생했는지에 대한 정보를 주지 않고, 선언이 현재 더 이상 사용되지 않는다는 것을 가리킨다. 버전 번호를 생략한다면, 콜론 (`:`) 또한 생략한다.

- `obsoleted` 인자는 선언이 쓸모 없어진 특정 플랫폼이나 언어의 최초 버전을 가리킨다. 선언이 쓸모 없어지면, 특정 플랫폼이나 언어에서 제거되고 더 이상 사용될 수 없다. 다음의 형식을 갖는다.

  ```swift
  obsoleted: {version number}
  ```

  *version number*는 구두점으로 구분된 한 개에서 세 개 까지의 양의 정수로 구성된다.

- `message` 인자는 deprecated 또는 obsoleted 선언을 사용한 것에 대한 경고 또는 에러를 낼 때 컴파일러가 표시할 텍스트 형식의 메세지를 제공한다. 다음의 형식을 갖는다.

  ```swift
  message: {message}
  ```

  *message*는 문자열 리터럴로 구성된다.

- `renamed` 인자는 이름이 바뀐 선언에 대해 새로운 이름을 가리키는 텍스트 형식의 메세지를 제공한다. 컴파일러는 이름이 바뀐 선언을 사용하는 것에 대해 에러를 낼 때 새로운 이름을 표시한다. 다음의 형식을 갖는다.

  ```swift
  renamed: {new name}
  ```

  *new name*은 문자열 리터럴로 구성된다.

  아래에서 보이는 것처럼, 프레임워크나 라이브러리의 릴리즈 간에 변경된 선언의 이름을 가리키기 위해, 타입 별칭 선언에 대해 `available` 애트리뷰트를 `renamed`와 `unavailable` 인자와 함께 적용할 수 있다. 이 조합은 선언의 이름이 바뀌었다는 컴파일 타임 에러를 낸다.

  ```swift
  // First release
  protocol MyProtocol {
      // protocol definition
  }
  // Subsequent release renames MyProtocol
  protocol MyRenamedProtocol {
      // protocol definition
  }
  
  @available(*, unavailable, renamed: "MyRenamedProtocol")
  typealias MyProtocol = MyRenamedProtocol
  ```

선언의 사용 가능성을 다른 플랫폼과 다른 Swift 버전에 대해 지정하기 위해 하나의 선언에 여러 개의 `available` 애트리뷰트를 적용할 수 있다. `available` 애트리뷰트가 적용된 선언은 애트리뷰트가 현재 타겟과 일치하지 않는 플랫폼이나 언어 버전을 지정한다면 무시된다. 여러 개의 `available` 애트리뷰트를 사용한다면, 효력이 있는 사용 가능성은 플랫폼과 Swift 사용 가능성의 조합이다.

`available` 애트리뷰트가 플랫폼이나 언어 이름 인자에 더하여 오직 `introduced` 인자만을 지정한다면, 다음의 간소화된 신택스를 대신 사용할 수 있다.

```swift
@available({platform name} {version number}, *)
@available(swift {version number})
```

`available` 애트리뷰트에 대한 간소화된 신택스는 여러 플랫폼에 대한 사용 가능성을 간결하게 표현한다. 두 개의 형식이 기능적으로 동일할지라도, 간소화된 형식은 가능하다면 더 선호된다.

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
    // class definition
}
```

Swift 버전 번호를 사용하여 사용 가능성을 지정한 `available` 애트리뷰트는 선언의 플랫폼 사용 가능성을 추가적으로 지정할 수 없다. 대신, Swift 버전 사용 가능성과 하나 이상의 플랫폼 사용 가능성을 지정하기 위해 분리된 `available` 애트리뷰트를 사용한다.

```swift
@available(swift 3.0.2)
@available(macOS 10.12, *)
struct MyStruct {
    // struct definition
}
```

### discardableRresult

이 애트리뷰트를 함수나 메소드 선언에 적용하여, 값을 반환하는 함수나 메소드가 그 결과를 사용하지 않고 호출되었을 때 컴파일러 경고를 감춘다.

### dynamicCallable

이 애트리뷰트를 클래스, 구조체, 열거형, 또는 프로토콜에 적용하여 타입의 인스턴스를 호출 가능한 함수처럼 취급한다. 해당 타입은 반드시 `dynamicallyCall(withArguments:)` 메소드 또는 `dynamicallyCall(withKeywordArguments:)` 메소드를 구현하거나, 둘 다 구현해야 한다.

동적으로 호출 가능한 타입의 인스턴스를 여러 개의 인자를 취하는 함수인 것처럼 호출할 수 있다.

```swift
@dynamicCallable
struct TelephoneExchange {
    func dynamicallyCall(withArguments phoneNumber: [Int]) {
        if phoneNumber == [4, 1, 1] {
            print("Get Swift help on forums.swift.org")
        } else {
            print("Unrecognized number")
        }
    }
}

let dial = TelephoneExchange()

// Use a dynamic method call.
dial(4, 1, 1)
// Prints "Get Swift help on forums.swift.org"

dial(8, 6, 7, 5, 3, 0, 9)
// Prints "Unrecognized number"

// Call the underlying method directly.
dial.dynamicallyCall(withArguments: [4, 1, 1])
```

`dynamicallyCall(withArguments:)` 메소드의 선언은 반드시 `ExpressibleByArrayLiteral` 프로토콜을 준수하는 하나의 매개변수를 가져야 한다. 위의 예시에서는 `[Int]`와 같은 것이다. 반환형은 어떤 타입이든 될 수 있다.

`dynamicallyCall(withKeywordArguments:)` 메소드를 구현한다면 동적 메소드 호출에 레이블을 포함할 수 있다.

```swift
@dynamicCallable
struct Repeater {
    func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
        return pairs
            .map { label, count in
                repeatElement(label, count: count).joined(separator: " ")
            }
            .joined(separator: "\n")
    }
}

let repeatLabels = Repeater()
print(repeatLabels(a: 1, b: 2, c: 3, b: 2, a: 1))
// a
// b b
// c c c
// b b
// a
```

`dynamicallyCall(withKeywordArguments:)` 메소드의 선언은 반드시 `ExpressibleByDictionaryLiteral` 프로토콜을 준수하는 하나의 매개변수를 가져야 하며, 반환형은 어떤 타입이든 될 수 있다. 매개변수의 `Key`는 반드시 `ExpressibleByStringLiteral`이어야 한다. 이전의 예시는 매개변수 타입으로 `KeyValuePairs`를 사용하여 호출자가 중복된 매개변수 레이블을 포함할 수 있다-`a`와 `b`는 `repeat`에 대한 호출에서 여러 번 나타난다.

`dynamicallyCall` 메소드 두 개를 모두 구현한다면, `dynamicallyCall(withKeywordArguments:)` 메소드는 메소드 호출이 키워드 인자를 포함할 때 호출된다. 다른 모든 케이스들에서는 `dynamicallyCall(withArguments:)`가 호출된다.

지정한 인자와 반환 값이 `dynamicallyCall` 메소드 구현 중 하나에 지정한 것과 일치할 때만 동적으로 호출 가능한 인스턴스를 호출할 수 있다. 다음의 예시에 있는 호출은 `KeyValuePairs<String, String>`을 취하는 `dynamicallyCall(withArguments:)`의 구현이 없으므로 컴파일되지 않는다.

```swift
repeatLabels(a: "four") // Error
```

### dynamicMemberLookup

이 애트리뷰트를 클래스, 구조체, 열거형, 또는 프로토콜에 적용하여 멤버들이 런타임에서 이름으로 룩업될 수 있게 한다. 해당 타입은 반드시 `subscript(dynamicMemberLookup:)` 서브스크립트를 구현해야 한다.

명시적인 멤버 표현식에서, 이름 있는 멤버에 대해 대응하는 선언이 없으면 해당 표현식은 해당 타입의 `subscript(dynamicMemberLookup:)` 서브스크립트의 호출로 이해되어, 멤버에 대한 정보를 인자로 넘긴다. 해당 서브스크립트는 키 경로 또는 멤버 이름인 매개변수를 받을 수 있다; 두 가지 모두의 서브스크립트를 구현한다면, 키 경로 인자를 취하는 서브스크립트가 사용된다.

`subscript(dynamicMemberLookup:)`의 구현은 `KeyPath`, `WritableKeyPath`, 또는 `ReferenceWritableKeyPath` 타입의 인자를 사용한 키 경로를 받을 수 있다. `ExpressibleByStringLiteral` 프로토콜을 준수하는 타입의 인자를 사용한 멤버 이름을 받을 수 있으며, 대부분의 경우 `String`이다. 서브스크립트의 반환형은 어떤 타입이든 될 수 있다.

멤버 이름에 의한 동적 멤버 룩업은, 다른 언어에서 Swift로 데이터를 브릿징하는 때처럼, 컴파일 타임에 타입이 확인될 수 없는 데이터 주변에 래퍼 타입을 만들기 위해 사용될 수 있다.

```swift
@dynamicMemberLookup
struct DynamicStruct {
    let dictionary = ["someDynamicMember": 325,
                      "someOtherMember": 787]
    subscript(dynamicMember member: String) -> Int {
        return dictionary[member] ?? 1054
    }
}
let s = DynamicStruct()

// Use dynamic member lookup.
let dynamic = s.someDynamicMember
print(dynamic)
// Prints "325"

// Call the underlying subscript directly.
let equivalent = s[dynamicMember: "someDynamicMember"]
print(dynamic == equivalent)
// Prints "true"
```

키 경로에 의한 동적 멤버 룩업은, 컴파일 타임 타입 확인을 지원하는 방식에서 래퍼 타입을 구현하기 위해 사용될 수 있다.

```swift
struct Point { var x, y: Int }

@dynamicMemberLookup
struct PassthroughWrapper<Value> {
    var value: Value
    subscript<T>(dynamicMember member: KeyPath<Value, T>) -> T {
        get { return value[keyPath: member] }
    }
}

let point = Point(x: 381, y: 431)
let wrapper = PassthroughWrapper(value: point)
print(wrapper.x)
```

### frozen

이 애트리뷰트를 구조체나 열거형 선언에 적용하여 타입에 대해 만들 수 있는 변화의 종류를 제한한다. 이 애트리뷰트는 오직 라이브러리 진화 모드에서 컴파일할 때만 허용된다. 라이브러리의 향후 버전은 열거형의 케이스나 구조체의 저장 인스턴스 프로퍼티를 추가, 삭제, 또는 순서를 바꾸어 선언을 변경할 수 없다. 이러한 변화들은 nonfrozen 타입에 대해서 허용되나, 그것들은 frozen 타입에 대해 ABI 호환성을 부순다.

- 알아두기

  컴파일러가 라이브러리 진화 모드가 아닐 때, 모든 구조체나 열거형은 암시적으로 frozen이며, 이 애트리뷰트는 무시된다.

라이브러리 진화 모드에서, nonfrozen 구조체와 열거형의 멤버와 상호작용하는 코드는 해당 라이브러리의 향후 버전이 그러한 타입의 멤버를 추가, 삭제, 또는 순서를 바꿀지라도 다시 컴파일하지 않고 작동하는 것을 지속하게 하는 방식으로 컴파일된다. 컴파일러는 런타임에서 정보를 찾고 간접 레이어에 추가하는 것과 같은 테크닉을 사용하여 이를 가능하게 한다. 구조체나 열거형을 frozen으로 표시하여 성능을 얻고 이러한 유연함을 포기한다: 라이브러리의 향후 버전은 해당 타입에 대해 오직 제한된 변경만을 할 수 있으나, 컴파일러는 해당 타입의 멤버들과 상호작용하는 코드에 대해 추가적으로 최적화할 수 있다.

frozen 타입, frozen 구조체의 저장 프로퍼티의 타입, 그리고 frozen 열거형 케이스의 연관 값들은 반드시 public이거나`usableFromInline` 애트리뷰트로 표시되어야 한다. frozen 구조체의 프로퍼티들은 프로퍼티 옵저버를 가질 수 없고, 저장 인스턴스 프로퍼티에 초기값을 제공하는 표현식은 반드시 인라이닝 가능한 함수와 같은 제한을 따라야 한다.

커맨드 라인에서 라이브러리 진화 모드를 활성화하기 위해 Swift 컴파일러에 `-enable-library-evolution` 옵션을 넘긴다. Xcode에서 활성화하려면 "Build Libraries for Distribution" 빌드 세팅 (`BUILD_LIBRARY_FOR_DISTRIBUTION`) 을 Yes로 설정한다.

frozen 열거형에서 switch 구문은 `default` 케이스를 요구하지 않는다. frozen 열거형에 대해 `default` 또는 `@unknown default` 케이스를 포함하는 것은, 해당 코드는 절대 실행되지 않으므로 경고를 낸다.

### GKInspectable

이 애트리뷰트를 적용하여 SpriteKit 에디터 UI에 커스텀 GameplayKit 컴포넌트 프로퍼티를 노출한다. 이 애트리뷰트를 적용하는 것은 또한 `objc` 애트리뷰트를 암시한다.

### inlinable

이 애트리뷰트를 함수, 메소드, 연산 프로퍼티, 서브스크립트, 편의 이니셜라이저, 또는 디이니셜라이저 선언에 적용하여 해당 선언의 구현을 모듈의 public 인터페이스의 일부분으로 노출한다. 컴파일러는 인라이닝 가능한 심볼에 대한 호출을 호출 지점에 있는 심볼의 구현의 복사본으로 교체할 수 있게 허용된다.

인라이닝 가능한 코드는 어떤 모듈에서 선언된 `public` 심볼과 함께 상호작용할 수 있고, 같은 모듈에 선언되어 `usableFromInline` 애트리뷰트와 함께 표시된 `internal` 심볼과 함께 상호작용할 수 있다. 인라이닝 가능한 코드는 `private` 또는 `fileprivate` 심볼과 상호작용할 수 없다.

이 애트리뷰트는 중첩된 내부 함수의 선언이나 `fileprivate` 또는 `private` 선언에 적용될 수 없다. 인라이닝 가능한 함수 안에서 정의된 함수와 클로저는 이 애트리뷰트로 표시되지 않았을지라도 암시적으로 인라이닝 가능하다.

### main

이 애트리뷰트를 구조체, 클래스, 또는 열거형 선언에 적용하여 이것이 프로그램 흐름을 위한 최상위 수준 엔트리 포인트를 포함한다는 것을 가리킨다. 해당 타입은 반드시 어떠한 인자도 취하지 않고 `Void`를 반환하는 `main` 타입 함수를 제공해야 한다.

```swift
@main
struct MyTopLevel {
    static func main() {
        // Top-level code goes here
    }
}
```

`main` 애트리뷰트의 요구조건을 기술하는 또다른 방법은 이 애트리뷰트를 작성한 타입이 반드시 다음의 가정된 프로토콜을 준수하는 타입처럼 같은 요구조건을 충족하게 해야 하는 것이다.

```swift
protocol ProvidesMain {
    static func main() throws
}
```

실행 가능한 파일을 만들기 위해 컴파일하는 Swift 코드는 최대 한 개의 최상위 수준 엔트리 포인트를 포함할 수 있다.

### nonobjc

이 애트리뷰트를 메소드, 프로퍼티, 서브스크립트, 또는 이니셜라이저 선언에 적용하여 암시적인 `objc` 애트리뷰트를 억제한다. `nonobjc` 애트리뷰트는 컴파일러에게 해당 선언이 Objective-C에서 그것을 표현하는 것이 가능할지라도 Objective-C 코드에서 사용할 수 없게 하라고 말한다.

이 애트리뷰트를 익스텐션에 적용하는 것은 명시적으로 `objc` 애트리뷰트로 표시하지 않은 익스텐션의 모든 멤버에 이를 적용하는 것과 같은 효과를 낸다.

클래스에서 `objc` 애트리뷰트로 표시된 브릿징 메소드에 대한 순환성을 해결하고, `objc` 애트리뷰트로 표시된 클래스에 있는 메소드와 이니셜라이저에 대한 오버로딩을 허용하기 위해 `nonobjc` 애트리뷰트를 사용한다.

`nonobjc` 애트리뷰트로 표시된 메소드는 `objc` 애트리뷰트로 표시된 메소드를 재정의할 수 없다. 그러나, `objc` 애트리뷰트로 표시된 메소드는 `nonobjc` 애트리뷰트로 표시된 메소드를 재정의할 수 있다. 비슷하게, `nonobjc` 애트리뷰트로 표시된 메소드는 `objc` 애트리뷰트로 표시된 메소드에 대한 프로토콜 요구조건을 충족할 수 없다.

### NSApplicationMain

이 애트리뷰트를 클래스에 적용하여 이것이 애플리케이션 델리게이트라는 것을 가리킨다. 이 애트리뷰트를 사용하는 것은 `NSApplication(_:_:)` 함수를 호출하는 것과 동일하다.

이 애트리뷰트를 사용하지 않는다면, `NSApplication(_:_:)` 함수를 호출하는 최상위 수준의 코드가 있는 `main.swift` 파일을 제공한다.

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```

실행 가능한 파일을 만들기 위해 컴파일하는 Swift 코드는 최대 한 개의 최상위 수준 엔트리 포인트를 포함할 수 있다.

### NSCopying

이 애트리뷰트를 클래스의 저장 변수 프로퍼티에 적용한다. 이 애트리뷰트는 프로퍼티의 설정자가 프로퍼티 그 자체의 값 대신 프로퍼티의 값의 *복사본*-`copyWithZone(_:)` 메소드가 반환하는 값-과 합성되도록 한다. 해당 프로퍼티의 타입은 반드시 `NSCopying` 프로토콜을 준수해야 한다.

`NSCopying` 애트리뷰트는 Objective-C의 `copy` 프로퍼티 애트리뷰트와 비슷한 방식으로 동작한다.

### NSManaged

이 애트리뷰트를 `NSManagedObject`로부터 상속받은 클래스의 인스턴스 메소드나 저장 변수 프로퍼티에 적용하여, 연관된 엔티티 디스크립션을 기반으로 Core Data가 런타임에서 그 구현을 동적으로 제공할 것임을 가리킨다. `NSManaged` 애트리뷰트로 표시된 프로퍼티에 대하여, Core Data는 또한 런타임에서 스토리지를 제공한다. 이 애트리뷰트를 적용하는 것은 또한 `objc` 애트리뷰트를 암시한다.

### objc

이 애트리뷰트를 어떠한 선언에 적용하여 이것이 Objective-C에서 표현될 수 있게 할 수 있다. 중첩되지 않은 클래스, 프로토콜, 제네릭이 아닌 열거형 (정수형 원시 값 타입에 한함), 클래스의 프로퍼티와 메소드 (접근자와 설정자를 포함함), 프로토콜과 프로토콜의 선택적 멤버들, 이니셜라이저, 서브스크립트에 적용할 수 있다. `objc` 애트리뷰트는 컴파일러에게 해당 선언이 Objective-C 코드에서 사용 가능하다고 말한다.

이 애트리뷰트를 익스텐션에 적용하는 것은 명시적으로 `nonobjc` 애트리뷰트로 표시되지 않은 익스텐션으 모든 멤버들에 적용하는 것과 같은 효과를 낸다.

컴파일러는 Objective-C에서 정의된 클래스들의 서브클래스에 암시적으로 `objc` 애트리뷰트를 추가한다. 그러나, 서브클래스는 반드시 제네릭이 아니어야 하고, 제네릭 클래스로부터 상속받지 않아야 한다. 이러한 기준을 충족하는 서브클래스들에게 아래에 있는 Objective-C 이름을 지정하여 `objc` 애트리뷰트를 명시적으로 추가할 수 있다. `objc` 애트리뷰트로 표시된 프로토콜들은 이 애트리뷰트로 표시되지 않은 프로토콜로부터 상속받을 수 없다.

`objc` 애트리뷰트는 다음의 케이스들에서 또한 암시적으로 추가된다.

- 선언이 서브클래스에서의 재정의이며, 슈퍼클래스의 해당 선언은 `objc` 애트리뷰트를 갖고 있다.
- 선언이 `objc` 애트리뷰트를 가진 프로토콜의 요구조건을 충족한다.
- 선언이 `IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, `IBInspectable`, `NSManaged`, 또는 `GKInspectable` 애트리뷰트를 갖고 있다.

열거형에 `objc` 애트리뷰트를 적용하면, 각각의 열거형 케이스는 열거형 이름과 케이스 이름을 이어 붙은 형태로 Objective-C 코드에 노출된다. 케이스 이름의 첫 번째 문자는 대문자가 된다. 예를 들어, Swift의 `Planet` 열거형에 있는 `venus` 케이스는 Objective-C 코드에서 `PlanetVenus`라는 이름의 케이스로 노출된다.

`objc` 애트리뷰트는 선택적으로 하나의 애트리뷰트 인자를 받으며, 이는 식별자로 구성되어 있다. 해당 식별자는 `objc` 애트리뷰트로 적용된 엔티티에 대해 Objective-C에 노출되는 이름을 지정한다. 이 인자를 사용하여 클래스, 열거형, 열거형 케이스, 프로토콜, 메소드, 접근자, 설정자, 이니셜라이저에 이름을 지어줄 수 있다. 클래스, 프로토콜, 또는 열거형에 대해 Objective-C 이름을 지정한다면, 세 개의 문자의 접두어를 이름에 포함한다. 아래의 예시는 `ExampleClass`의 `enabled` 프로퍼티의 접근자를 Objective-C 코드에 노출하면서 해당 프로퍼티 자체의 이름이 아닌 `isEnabled`로 노출하고 있다.

```swift
class ExampleClass: NSObject {
    @objc var enabled: Bool {
        @objc(isEnabled) get {
            // Return the appropriate value
        }
    }
}
```

- 알아두기

  `objc` 애트리뷰트의 인자는 해당 선언에 대해 런타임 이름도 바꿀 수 있다. `NSClassFromString`처럼, Objective-C 런타임에서 상호작용하는 함수를 호출하고, 앱의 Info.plist 파일에 클래스 이름을 지정할 때 런타임 이름을 사용한다. 인자를 넘겨서 이름을 지정한다면, 해당 이름은 Objective-C 코드에서의 이름과 런타임 이름으로 사용된다. 해당 인자를 생략한다면, Objective-C 코드에서 사용되는 이름은 Swift 코드의 이름과 일치하며, 런타임 이름은 name mangling에 대한 일반적인 Swift 컴파일러 컨벤션을 따른다.

### objcMembers

이 애트리뷰트를 클래스 선언에 적용하여, 클래스와 그것의 익스텐션, 서브클래스, 그리고 서브클래스의 모든 익스텐션의 Objective-C와 호환되는 모든 멤버에 대해 `objc` 애트리뷰트를 암시적으로 적용한다.

대부분의 코드는 오직 필요로 하는 선언만 노출하기 위해 `objc` 애트리뷰트를 대신 사용할 필요가 있다. 많은 선언을 노출할 필요가 있다면, `objc` 애트리뷰트를 갖는 익스텐션에 그것들을 모아둘 수 있다. `objcMembers` 애트리뷰트는 Objective-C 런타임의 내성 설비를 많이 사용하는 라이브러리에 대한 편의다. 필요하지 않을 때 `objc` 애트리뷰트를 적용하는 것은 바이너리 크기를 증가시키고 성능에 불리하게 작용할 수 있다.

### propertyWrapper

이 애트리뷰트를 클래스, 구조체, 또는 열거형 선언에 적용하여 해당 타입을 프로퍼티 래퍼로 사용한다. 이 애트리뷰트를 타입에 적용할 때, 타입과 같은 이름을 가진 커스텀 애트리뷰트를 만든다. 이 새로운 애트리뷰트를 클래스, 구조체, 또는 열거형의 프로퍼티에 적용하여 래퍼 타입의 인스턴스를 통해 프로퍼티에 대한 접근을 감싼다. 지역 및 전역 변수는 프로퍼티 래퍼를 사용할 수 없다.

해당 래퍼는 반드시 `wrappedValue` 인스턴스 프로퍼티를 정의해야 한다. 프로퍼티의 *wrapped value*는 해당 프로퍼티가 노출하는 접근자와 설정자에 대한 값이다. 대부분의 경우, `wrappedValue`는 연산된 값이지만, 대신 저장된 값도 될 수 있다. 래퍼는 그것의 래핑된 값이 필요로 하는 어떠한 기반 스토리지를 정의하고 관리할 책임이 있다. 컴파일러는 언더스코어 (`_`) 를 래핑된 프로퍼티의 이름 앞에 붙여서 래퍼 타입의 인스턴스에 대한 스토리지를 합성한다. 예를 들어, `someProperty`에 대한 래퍼는 `_someProperty` 처럼 저장된다. 래퍼를 위한 합성된 스토리지는 `private` 접근 제어 수준을 갖는다.

프로퍼티 래퍼를 갖는 프로퍼티는 `willSet`과 `didSet` 블록을 포함할 수 있으나, 컴파일러가 합성한 `get` 또는 `set` 블록은 재정의할 수 없다.

Swift는 프로퍼티 래퍼의 초기화에 대해 두 가지 형식의 신택틱 슈거를 제공한다. 래핑된 값의 정의에 할당문 신택스를 사용하여 할당문의 우측에 있는 표현식을 프로퍼티 래퍼의 이니셜라이저의 `wrappedValue` 매개변수에 인자로 넘겨줄 수 있다. 또한 이것을 프로퍼티에 적용할 때 애트리뷰트에 인자를 제공할 수 있고, 그러한 인자들은 프로퍼티 래퍼의 이니셜라이저에 전달된다. 예를 들어, 아래의 코드에서 `SomeStruct`는 `SomeWrapper`가 정의한 각각의 이니셜라이저를 호출한다.

```swift
@propertyWrapper
struct SomeWrapper {
    var wrappedValue: Int
    var someValue: Double
    init() {
        self.wrappedValue = 100
        self.someValue = 12.3
    }
    init(wrappedValue: Int) {
        self.wrappedValue = wrappedValue
        self.someValue = 45.6
    }
    init(wrappedValue value: Int, custom: Double) {
        self.wrappedValue = value
        self.someValue = custom
    }
}

struct SomeStruct {
    // Uses init()
    @SomeWrapper var a: Int

    // Uses init(wrappedValue:)
    @SomeWrapper var b = 10

    // Both use init(wrappedValue:custom:)
    @SomeWrapper(custom: 98.7) var c = 30
    @SomeWrapper(wrappedValue: 30, custom: 98.7) var d
}
```

래핑된 프로퍼티에 대한 *projected value*는 프로퍼티 래퍼가 추가 기능을 노출하기 위해 사용할 수 있는 두 번째 값이다. 프로퍼티 래퍼 타입의 작성자는 이 투영된 값의 의미를 결정하고, 해당 투영된 값이 노출할 인터페이스를 정의할 책임이 있다. 프로퍼티 래퍼에서 값을 투영하기 위해, 래퍼 타입의 인스턴스 프로퍼티로 `projectedValue`를 정의한다. 컴파일러는 래핑된 프로퍼티의 이름에 달러 기호 (`$`) 를 앞에 붙여서 투영된 값에 대한 식별자를 합성한다. 예를 들어, `someProperty`의 투영된 값은 `$someProperty`다. 투영된 값은 기존 래핑된 프로퍼티와 같은 접근 제어 수준을 갖는다.

```swift
@propertyWrapper
struct WrapperWithProjection {
    var wrappedValue: Int
    var projectedValue: SomeProjection {
        return SomeProjection(wrapper: self)
    }
}
struct SomeProjection {
    var wrapper: WrapperWithProjection
}

struct SomeStruct {
    @WrapperWithProjection var x = 123
}
let s = SomeStruct()
s.x           // Int value
s.$x          // SomeProjection value
s.$x.wrapper  // WrapperWithProjection value
```

### requires_stored_property_inits

이 애트리뷰트를 클래스 선언에 적용하여 클래스 내의 모든 저장 프로퍼티들에게 그 정의의 일부분으로 기본값을 제공해야 하게 한다. 이 애트리뷰트는 `NSManagedObject`로부터 상속받은 클래스들에 대해 추론된다.

### testable

이 애트리뷰트를 `import` 선언에 적용하여 해당 모듈의 접근 수준을 변경하여 모듈의 코드를 테스트하는 것을 단순화하여 임포트한다. `internal` 접근 수준 지정자로 표시된 임포트된 모듈의 엔티티들은 `public` 접근 수준 지정자로 선언된 것처럼 임포트된다. `internal`이나 `public` 접근 수준 지정자로 표시된 클래스와 클래스 멤버들은 `open` 접근 수준 지정자로 선언된 것처럼 임포트된다. 임포트된 모듈은 반드시 테스트가 활성화되었을 때 반드시 컴파일되어야 한다.

### UIApplicationMain

이 애트리뷰트를 클래스에 적용하여 이것이 애플리케이션 델리게이트라는 것을 가리킨다. 이 애트리뷰트를 사용하는 것은 `UIApplicationMain` 함수를 호출하고 이 클래스의 이름을 델리게이트 클래스의 이름으로 넘기는 것과 동일하다.

이 애트리뷰트를 사용하지 않는다면, `UIApplication(_:_:_:_:)` 함수를 최상위 수준에서 호출하는 코드를 가진 `main.swift` 파일을 제공한다. 예를 들어, 앱이 그것의 주요 클래스로 `UIApplication`의 커스텀 서브클래스를 사용한다면, 이 애트리뷰트를 사용하는 대신 `UIApplication(_:_:_:_:)` 함수를 호출한다.

실행 가능한 파일을 만들기 위해 컴파일하는 Swift 코드는 최대 한 개의 최상위 수준 엔트리 포인트를 포함할 수 있다.

### usableFromInline

이 애트리뷰트를 함수, 메소드, 연산 프로퍼티, 서브스크립트, 이니셜라이저, 또는 디이니셜라이저 선언에 적용하여 해당 심볼이 해당 선언과 동일한 모듈에 정의된 인라이닝 가능한 코드에서 사용될 수 있게 한다. 해당 선언은 반드시 `internal` 접근 수준 지정자를 가지고 있어야 한다. `usableFromInline`으로 표시된 구조체나 클래스는 오직 public이거나 해당 프로퍼티에 대해 `usableFromInline`인 타입만을 사용할 수 있다. `usableFromInline`으로 표시된 열거형은 오직 public이거나 해당 케이스들의 원시 값과 연관 값에 대해 `usableFromInline`인 타입만을 사용할 수 있다.

`public` 접근 수준 지정자처럼, 이 애트리뷰트는 해당 선언을 모듈의 공개 인터페이스의 일부분으로 노출한다. `public`과는 다르게, 컴파일러는 `usableFromInline`으로 표시된 선언이, 해당 선언의 심볼이 익스포트되었을지라도, 해당 모듈 바깥에서 코드의 이름으로 참조되는 것을 허용하지 않는다. 그러나 모듈 바깥에 있는 코드는 런타임 동작을 사용하여 여전히 해당 선언의 심볼과 상호작용할 수 있을 것이다.

`inlinable` 애트리뷰트로 표시된 선언은 암시적으로 인라이닝 가능한 코드에서 사용 가능하다. `inlinable` 또는 `usableFromInline`이 `internal` 선언에 적용 가능할지라도, 두 애트리뷰트를 모두 적용하는 것은 에러다.

### warn_unqualified_access

이 애트리뷰트를 최상위 수준 함수, 인스턴스 메소드, 또는 클래스 또는 정적 메소드에 적용하여 해당 함수나 메소드가 모듈 이름, 타입 이름, 또는 인스턴스 변수 또는 상수와 같은 선행 수식어 없이 사용될 때 경고를 내도록 한다. 이 애트리뷰트를 사용하여 동일한 스코프에서 접근할 수 있는 같은 이름을 가진 함수들 간의 모호함을 없애는 것을 돕는다.

예를 들어, Swift 표준 라이브러리는 최상위 수준의 `min(_:_:)` 함수와 비교 가능한 요소들이 있는 시퀀스에 대한 `min()` 메소드를 포함한다. 시퀀스 메소드는 `warn_unqualified_access` 애트리뷰트와 함께 선언되어 `Sequence` 익스텐션에서 둘 중 하나를 사용하려고 할 때의 혼란을 줄일 수 있게 도와준다.

### 인터페이스 빌드에서 사용되는 선언 애트리뷰트

인터페이스 빌더 애트리뷰트는 Xcode와 동기화하기 위해 인터페이스 빌더가 사용하는 선언 애트리뷰트다. Swift는 `IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, `IBInspectable` 인터페이스 빌더 애트리뷰트를 제공한다. 이러한 애트리뷰트들은 Objective-C와 짝을 이루는 것들과 개념적으로 동일하다.

클래스의 프로퍼티 선언에 `IBOutlet`과 `IBInspectable` 애트리뷰트를 적용할 수 있다. 클래스의 메소드 선언에 `IBAction`과 `IBSegueAction` 애트리뷰트를 적용할 수 있고, 클래스 선언에 `IBDesignabled` 애트리뷰트를 적용할 수 있다.

`IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, 또는 `IBInspectable` 애트리뷰트를 적용하는 것은 또한 `objc` 애트리뷰트를 암시한다.

## 타입 애트리뷰트

타입들에 한해서만 타입 애트리뷰트를 적용할 수 있다.

### autoclosure

이 애트리뷰트를 적용하여 인자가 없는 클로저에 표현식을 자동으로 래핑하여 표현식의 평가를 늦춘다. 메소드나 함수의 선언에 있는 매개변수의 타입에 이것을 적용하며, 매개변수는 그것의 타입이 인자를 취하지 않는 함수 타입이고 해당 표현식의 타입의 값을 반환한다.

### convention

이 애트리뷰트를 함수의 타입에 적용하여 그것의 호출 컨벤션을 가리킨다.

`convention` 애트리뷰트는 항상 다음의 인자 중 하나가 따라나온다.

- `swift` 인자는 Swift 함수 레퍼런스를 가리킨다. 이는 Swift의 함수 값에 대한 표준 호출 컨벤션이다.
- `block` 인자는 Objective-C와 호환되는 블록 레퍼런스를 가리킨다. 해당 함수 값은 블록 객체에 대한 참조로 표현되며, 이는 객체 내의 호출*invocation* 함수를 내장한 `id` 호환되는 Objective-C 객체다. 해당 호출 함수는 C 호출 컨벤션을 사용한다.
- `c` 인자는 C 함수 레퍼런스를 가리킨다. 해당 함수 값은 컨텍스트를 가져오지 않고 C 호출 컨벤션을 사용한다.

몇 가지 예외를 제외하고, 다른 호출 규칙을 필요로 하는 함수가 있을 때 어떠한 호출 컨벤션을 가진 함수든 사용할 수 있다. 제네릭이 아닌 전역 함수와, 지역 변수나 지역 변수를 획득하지 않는 클로저를 획득하지 않는 제네릭이 아닌 지역 함수는, C 호출 컨벤션으로 전환될 수 있다. 다른 Swift 함수는 C 호출 컨벤션으로 전환될 수 없다. Objective-C 블록 호출 컨벤션을 가진 함수는 C 호출 컨벤션으로 전환될 수 없다.

### escaping

이 애트리뷰트를 메소드나 함수의 선언에 있는 매개변수의 타입에 적용하여 해당 매개변수의 값이 나중의 실행을 위해 저장될 수 있다는 것을 가리킨다. 이는 해당 값이 호출의 수명이 다하고도 살아남을 수 있게 한다는 것을 의미한다. `escaping` 타입 애트리뷰트를 가진 함수 타입 매개변수는 프로퍼티나 메소드에 대해 명시적으로 `self.`를 사용해야 한다.

### 스위치 케이스 애트리뷰트

케이스를 스위치할 때만 스위치 케이스 애트리뷰트를 적용할 수 있다.

### unknown

이 애트리뷰트를 스위치 케이스에 적용하여 코드가 컴파일되는 시점에서 알려진 열거형의 케이스들과 일치하지 않을 수 있다는 것을 의미한다.