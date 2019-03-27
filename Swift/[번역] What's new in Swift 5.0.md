[원문: Hacking with Swift](https://www.hackingwithswift.com/articles/126/whats-new-in-swift-5-0)

# Swift 5.0에서 새로워진 것

Swift 5.0은 Swift의 다음 주요 릴리즈이며, 마침내 ABI 안정성을 가져왔습니다. 하지만 이것이 다가 아닙니다. 몇 가지 주요한 새로운 기능이 이미 구현되었는데, 이는 원시 문자열*raw strings*, 미래의 열거형 케이스*future enum cases*, `Result` 타입, 정수형 배수 확인*checking for integer multiples* 등을 포함합니다.

## 표준 `Result` 타입*Standard Result Type*

SE-0235는 표준 라이브러리에 `Result` 타입을 도입하였습니다. 이는 비동기 API와 같은 복잡한 코드에서 더 간단하고 명확한 에러 처리 방식을 제공합니다.

Swift의 `Result` 타입은 두 가지 케이스 `success` 및 `failure`를 가진 열거형으로 구현되었습니다. 둘 모두는 제네릭을 사용하여 구현되어서, 당신의 선택에 따른 연관 값을 가질 수 있습니다. 하지만, `failure`는 Swift의 `Error` 타입을 준수하는 타입이 되어야 합니다.

`Result`를 시연하기 위해, 사용자를 기다리고 있는 읽지 않은 메세지가 얼마나 많은지 알기 위해 서버와 연결하는 함수를 작성할 수 있을 것입니다. 이 예제 코드에서 우리는 오직 하나의 가능한 에러만을 가질 것이며, 이는 요청 URL 문자열이 유효한 URL이 아님을 나타냅니다.

```swift
enum NetworkError: Error {
  case badURL
}
```

가져오기 함수는 URL 문자열을 첫 번째 매개변수로, 컴플리션 핸들러를 두 번째 매개변수로 받아들일 것입니다. 그 컴플리션 핸들러는 그 자체가 `Result`를 받아들이며, 성공 케이스에서는 정수형을 저장할 것이고, 실패 케이스에서는 `NetworkError`의 한 종류가 될 것입니다. 우리는 여기에서 실제로 서버에 연결하려고 하지는 않을 것입니다. 하지만 비동기 코드를 흉내내기 위해 적어도 컴플리션 핸들러를 사용할 것입니다.

```swift
import Foundation

func fetchUnreadCount1(from urlString: String, completionHandler: @escaping (Result<Int, NetworkError>) -> Void) {
  guard let url = URL(string: urlString) else {
    completionHandler(.failure(.badURL))
    return
  }
  // 여기에 복잡한 네트워킹 코드가 위치함
  print("Fetching \(url.absoluteString)")
  completionHandler(.success(5))
}
```

이 코드를 사용하기 위해, 호출이 성공했는지 실패했는지 알기 위해 `Result`의 내부를 확인할 필요가 있습니다.

```swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in 
  switch result {
  case .success(let count):
    print("\(count) unread messages.")
  case .failure(let error):
    print(error.localizedDescription)
  }
}
```

당신의 코드에서 `Result`를 사용하기 전에 알아야 하는 세 가지 것들이 더 있습니다.

먼저, `Result`는 `get()` 메소드를 가지고 있으며, 이는 성공 케이스에서의 값을 가지고 있다면 그것을 반환하고, 그렇지 않으면 그것이 가지고 있는 에러를 던집니다. 이는 `Result`를 일반적인 예외를 던지는 호출의 형식으로 변환할 수 있게 해줍니다.

```swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in 
  if let count = try? result.get() {
    print("\(count) unread messages.")                                                             
  }
}
```

두 번째로, `Result`는 예외를 던지는 클로저를 받는 이니셜라이저를 가지고 있습니다. 그 클로저가 성공적으로 값을 반환한다면, 그것은 `success` 케이스를 위해 사용되며, 그렇지 않으면 `failure` 케이스에 그것이 던지는 에러가 위치하게 됩니다.

```swift
let result = Result { try String(contentsOfFile: someFile) }
```

세 번째로, 당신이 생성한 특정 에러 열거형을 사용하기보다는, 일반적인 `Error` 프로토콜도 사용할 수 있습니다. 사실, Swift Evolution 제안은 "Result의 대부분의 사용은 `Error` 타입 인자로 `Swift.Error`를 사용할 것으로 기대된다."라고 말합니다.

그래서, `Result<Int, NetworkError>`를 사용하기보다는, `Result<Int, Error>`를 사용할 수 있습니다. 이는 당신이 타입이 있는 예외 던지기의 안전성을 잃는 것을 의미할지라도, 다양한 에러 열거형을 던질 수 있는 능력을 얻게 됩니다. 이는 정말로 당신의 코딩 스타일에 따라 선호도가 결정되는 것입니다.

## 원시 문자열*Raw String*

SE-0200은 원시 문자열을 생성할 수 있는 능력을 추가하였는데, 백슬래시와 큰따옴표가 문자나 문자열 터미네이터를 탈출하는 대신 그 자체의 리터럴 심볼로 해석됩니다. 이것은 많은 유즈 케이스를 더 쉽게 만들지만, 특히 정규 표현식에서 도움이 될 것입니다.

원시 문자열을 사용하기 위해, 당신의 문자열 앞에 하나 또는 그 이상의 `#` 심볼을 위치시키십시오.

```swift
let rain = #"The "rain" in "Spain" fallas mainly on the Spaniards."#
```

문자열의 시작과 끝에 있는 `#` 심볼은 문자열 구분 기호*string delimiter*의 일부분이 됩니다. 그래서 Swift는 "rain"과 "Spain" 주위에 있는 독립된 큰따옴표가 문자열을 끝내는 대신 리터럴 큰따옴표로 처리하도록 하는 것으로 이해합니다.

원시 문자열은 또한 백슬래시를 사용하는 것도 허용합니다.

```swift
let keypaths = #"Swift keypaths such as \Person.name hold uninvoked references to properties."#
```

이는 백슬래시를 탈출 문자 대신 문자열 내에서 리터럴 문자가 되도록 처리합니다. 이는 문자열 보간법*string interpolation*이 다르게 동작한다는 것을 의미합니다.

```swift
let answer = 42
let dontpanic = #"The answer to life, the universe, and everything is \#(answer)."#
```

문자열 보간법을 사용하기 위해 `\#(answer)`를 어떻게 사용하였는지에 주목하십시오. 일반적인 `\(answer)`는 문자열 내에서 문자로 해석될 것입니다. 그러므로 원시 문자열 내에서 문자열 보간법을 사용하고 싶을 때, 여분의 `#`를 추가해 주어야 합니다.

Swift의 원시 문자열의 흥미로운 기능 중 하나는 시작과 끝에 해쉬 심볼을 사용한다는 것인데, 필요하다면 원하지 않는 이벤트에 대해서 하나 이상을 사용할 수 있기 때문입니다. 이는 극도로 희귀한 경우여야 하기 때문에 여기에서 좋은 예제를 제공하기 어렵지만, 다음의 문자열을 고려해 보십시오. **My dog said "woof"#gooddog**. 해쉬 심볼 이전에 공간이 없기 때문에, Swift는 `"#`를 볼 것이고 즉시 이것을 문자열 터미네이터로 해석할 것입니다. 이 상황에서 우리는 구분 문자를 `#"` 에서 `##"`로 변경할 필요가 있습니다.

```swift
let str = ##"My dog said "woof"#gooddog"##
```

끝에 있는 해쉬 심볼의 개수가 시작에 있는 해쉬 심볼의 개수와 어떻게 일치되어야 하는지에 주목하십시오.

원시 문자열은 Swift의 여러줄 문자열 체계*multi-line string system*과 완전하게 호환됩니다. 단지 시작 부분에 `#"""`, 끝 부분에 `"""#`을 사용하기만 하면 됩니다.

```swift
let multiline = #"""
The answer to life,
the universe,
and everything is \#(answer).
"""#
```

많은 백슬래시 없이 할 수 있다는 것은 정규 표현식에서 특히 유용하다는 것을 증명할 것입니다. 예를 들어, `\Person.name`과 같은 키 경로를 찾기 위해 간단한 정규 표현식을 작성하였습니다.

```swift
let regex1 = "\\\\[A-Z]+[A-Za-z]+\\.[a-z]+"
```

원시 문자열 덕분에 우리는 백슬래시를 반만 사용하고도 같은 것을 작성할 수 있습니다.

```swift
let regex2 = #"\\[A-Z]+[A-Za-z]+\.[a-z]+"#
```

우리는 여전히 *어떠한 것* *some*이 필요한데, 정규 표현식이 그것들도 사용하기 때문입니다.

## 문자열 보간법 커스터마이징하기*Customizing String Interpolation*

SE-0228은 Swift의 문자열 보간법 체계를 극적으로 쇄신하여, 더욱 능률적이고 더욱 유연하게 하였으며, 이전에는 불가능했던 완전히 새로운 기능의 범위를 만들고 있습니다.

가장 기초적인 형식에서, 새로운 문자열 보간법 체계는 오브젝트가 문자열로 표현되는 방법을 제어할 수 있게 해줍니다. Swift는 디버깅하는 데 도움을 주는, 구조체에 대한 기본 행동을 가지고 있는데, 문자열 이름과 그것이 가진 모든 프로퍼티를 이어서 출력하기 때문입니다. 하지만 클래스를 가지고 작업하고 있다면(이 행동을 가지고 있지 않음), 또는 사용자 중심적인 출력 포맷을 원할 때, 새로운 문자열 보간법 체계를 사용할 수 있을 것입니다.

```swift
struct User {
  var name: String
  var age: Int
}
```

사용자들을 산뜻하게 출력하기 위해 특별한 문자열 보간법을 추가하기 원한다면, `String.StringInterpolation`을 확장하여 새로운 `appendInterpolation()` 메소드를 추가할 수 있습니다. Swift는 이미 몇 가지의 내장된 형태를 가지고 있고, 그 보간 *타입*을 사용합니다. 이 경우 `User`는 어떠한 메소드를 호출할지 알아냅니다.

이 경우, 우리는 하나의 문자열에 사용자의 이름과 나이를 넣고, 그것을 우리의 문자열에 추가하기 위해 내장 `appendInterpolation()` 메소드 중 하나를 호출하는 구현을 추가할 것입니다.

```swift
extension String.StringInterpolation {
  mutating func appendInterpolation(_ value: User) {
    appendInterpolation("My name is \(value.name) and I'm \(value.age)")
  }
}
```

이제 우리는 사용자를 생성하고 그들의 데이터를 출력할 수 있습니다.

```swift
let user = User(name: "Guybrush Threepwood", age: 33)
print("User details: \(user)")
```

이는 **User details: My name is Guybrush Threepwood and I'm 33**을 출력할 것입니다. 반면에 커스텀 문자열 보간법으로 이는 **User details: User(name: "Guybrush Threepwood", age: 33)**을 출력할 것입니다. 물론, `CustomStringConvertible` 프로토콜을 구현하는 것과 기능의 차이는 없으니, 더 나아간 사용 방법을 알아봅시다.

당신의 커스텀 보간 메소드는 필요한 만큼 많은 매개변수를 취할 수 있으며, 인자 레이블이 있을 수도 없을 수도 있습니다. 예를 들어, 바양한 스타일을 사용하여 숫자를 출력하기 위해 보간을 추가할 수 있습니다.

```swift
extension String.StringInterpolation {
  mutating func appendInterpolation(_ number: Int, style: NumberFormatter.Style) {
    let formatter = Formatter()
    formatter.numberStyle = style
    if let result = formatter.string(from: number as NSNumber) {
      appendLiteral(result)
    }
  }
}
```

`NumberFormatter` 클래스는 통화*currency*, 서수, 기수*spell out*를 포함하는 다양한 스타일을 가지고 있습니다. 그래서 우리는 임의의 숫자를 생성하고 특정 문자열로 그것을 나타낼 수 있습니다.

```swift
let number = Int.random(in: 0...100)
let lucky = "The lucky number this week is \(number, style: .spellOut)."
print(lucky)
```

필요한 만큼 `appendLiteral()`을 호출할 수 있습니다. 필요하다면 전혀 사용하지 않을 수 있습니다. 예를 들어, 문자열을 여러 번 반복하기 위해 문자열 보간을 추가할 수 있을 것입니다.

```swift
extension String.StringInterpolation {
  mutating func appendInterpolation(repeat str: String, _ count: Int) {
    for _ in 0..<count {
      appendLiteral(str)
    }
  }
}
print("Baby shart \(repeat: "doo ", 6)")
```

그리고, 이것들은 단지 일반적인 메소드일 뿐이기 때문에, Swift의 모든 기능을 사용할 수 있습니다. 예를 들어, 우리는 문자열 배열을 함께 결합하지만, 배열이 비어 있다면 대신 문자열을 반환하는 클로저를 실행하는 보간을 추가할 수 있을 것입니다.

```swift
extension String.StringInterpolation {
  mutating func appendInterpolation(_ values: [String], empty defaultValue: @autoclosure () -> String) {
    if values.count == 0 {
      appendLiteral(defaultValue())
    } else {
      appendLiteral(values.joined(separator: ", "))
    }
  }
}

let names = ["Harry", "Ron", "Hermione"]
print("List of students: \(names, empty: "No one").")
```

`@autoclosure`를 사용하는 것은 기본값으로 간단한 값을 사용하거나 복잡한 함수를 호출할 수 있다는 것을 의미하나, 이러한 작업은 `values.count`가 0이 아니면 실행되지 않을 것입니다.

`ExpressibleByStringLiteral`과 `ExpressibleByStringInterpolation` 프로토콜의 조합으로 모든 타입이 문자열 보간법을 사용하는 것을 가능하게 할 수 있으며, `CustomStringConvertible`을 추가한다면 우리가 원하는 대로 그러한 유형들을 문자열로 출력할 수도 있습니다.

이 작업을 하기 위해, 몇 가지 특정 조건을 만족할 필요가 있습니다.

- 우리가 생성하는 타입은 `ExpressibleByStringLiteral`, `ExpressibleByStringInterpolation`, `CustomStringConvertible`을 준수해야 합니다. 후자는 타입이 출력되는 방법을 커스터마이징하기 원할 때에만 필요합니다.
- 당신의 타입 내에 `StringInterpolation` 프로토콜을 준수하는 `StringInterpolation` 중첩 구조체를 필요로 합니다.
- 중첩 구조체는 데이터의 크기를 어림잡아 기대할 수 있도록 우리에게 알려주는 두 개의 정수를 받는 이니셜라이저를 가질 필요가 있습니다.
- 그것은 또한 `appendLiteral()` 메소드, 하나 또는 그 이상의 `appendInterpolation()` 메소드의 구현을 필요로 합니다.
- 당신의 메인 타입은 문자열 리터럴과 문자열 보간으로부터 생성될 수 있게 해주는 두 개의 이니셜라이저를 가질 필요가 있습니다.

우리는 다양한 일반적인 요소로부터 HTML을 구축할 수 있는 예제 타입에 이 모든 것들을 넣을 수 있습니다. `StringInterpolation` 중첩 구조체에 있는 "보조 메모리"는 문자열이 될 것입니다. 새로운 리터럴이나 보간이 추가될 때마다, 그것을 그 문자열에 추가할 것입니다.

```swift
struct HTMLComponent: ExpressibleByStringLiteral, ExpressibleByStringInterpolation, CustomStringConvertible {
  struct StringInterpolation: StringInterpolationProtocol {
    // 비어 있는 문자열로 시작
    var output = ""
    
    // 리터럴 텍스트의 양의 두 배를 가지기 위해 충분한 공간 할당하기
    init(literalCapacity: Int, interpolationCount: Int) {
      output.reserveCapacity(literalCapacity * 2)
    }
    
    // 하드코딩된 텍스트 조각 - 단지 추가
    mutating func appendLiteral(_ literal: String) {
      print("Appending \(literal)")
      output.append(literal)
    }
    
    // 트위터 사용자 이름 - 링크로 추가
    mutating func appendInterpolation(twitter: String) {
      print("Appending \(twitter)")
      output.append("<a href=\"https://twitter/\(twitter)\">@\(twitter)</a>")
    }
    
    // 이메일 주소 - mailto 사용하여 추가
    mutating func appendInterpolation(email: String) {
      print("Appending \(email)")
      output.append("<a href=\"mailto:\(email)\">\(email)</a>")
    }
  }
  
  // 이 모든 컴포넌트에 대한 완성된 텍스트
  let description: String
  
  // 리터럴 문자열로부터 인스턴스 생성하기
  init(stringLiteral value: String) {
    description = value
  }
  
  // 보간된 문자열로부터 인스턴스 생성하기
  init(stringInterpolation: StringInterpolation) {
    description = stringInterpolation.output
  }
}
```

이제 문자열 보간법을 사용하여 `HTMLComponent`의 인스턴스를 생성하고 사용할 수 있습니다.

```swift
let text: HTMLComponent = "You should follow me on Twitter \(twitter: "twostraws"), or you can email me at \(email: "paul@hackingwithswift.com")."
```

산재해 있는 `print()` 호출 덕분에, 문자열 보간 기능이 어떻게 작동하는지 확실하게 알 수 있을 것입니다. "Appending You should follow me on Twitter", "Appending twostraws", "Appending, or you can email me at", "Appending paul@hackingwithswift.com", "Appending ."을 확인할 수 있을 것입니다. 각 부분은 메소드 호출을 일으키며, 우리의 문자열에 추가됩니다.

## 동적으로 호출 가능한 타입*Dynamically callable types*

SE-0216은 Swift에 새로운 `@dynamicCallable` 특성을 추가하였으며, 이는 해당 타입이 직접적으로 호출 가능하다는 것으로 표시할 수 있는 능력을 가져옵니다. 이것은 컴파일러의 마법의 한 종류라기보다는 syntatic sugar인데, 다음의 코드는 다음의 코드로 능률적으로 변형됩니다.

```swift
let result = random(numberOfZeroes: 3)
let result = random.dynamicallyCall(withKeywordArguments: ["numberOfZeros": 3])
```

이전에 Swift 4.2의 @dynamicMemberLookup에 대한 기능을 소개했습니다. `@dynamicCallable`은 `@dynamicMemberLookup`의 순수 확장이며, 같은 목적을 제공합니다. 이는 Swift 코드가 Python과 JavaScript와 같은 동적 언어와 나란히 작동하는 것을 쉽게 해줍니다.

이 기능을 당신의 타입에 추가하기 위해, `@dynamicCallable` 특성을 추가하고 다음의 하나 또는 둘 모드의 메소드를 추가할 필요가 있습니다.

```swift
func dynamicallyCall(withArguments args: [Int]) -> Double
func dynamicallyCall(withKeywordArguments args: KeyValuePairs<String, Int>) -> Double
```

