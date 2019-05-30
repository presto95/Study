[원문 : Hacking With Swift](https://www.hackingwithswift.com/articles/182/whats-new-in-swift-5-1)

# Swift 5.1에서 새로워진 것

Swift 5.0은 Xcode 10.2에서 소개되었으나, 작업은 수 개월 동안 그 후계자에서 진행중이며 이미 기대되는 많은 것들이 있습니다.

Swift 5.0에서처럼, 5.1은 모듈 안정성의 형태에서 주요한 특징을 가지고 있습니다. 이는 써드 파티 라이브러리가 만들어진 Swift 컴파일러의 버전을 걱정하지 않도록 해줍니다. 이는 Swift 5.0에서 획득한 ABI 안정성과 비슷하게 들리지만, 미묘한 차이점이 있습니다. ABI 안정성은 런타임에서 Swift의 차이를 해결하지만, 모듈 안정성은 컴파일 타임에서의 차이를 해결합니다.

주요한 이정표를 따라 이미 많은 중요한 언어의 향상을 획득하고 있습니다. 나는 이 글에서 그것들을 따라가고 실제로 그것들을 확인할 수 있도록 코드 예시를 제공할 것입니다.

## 통합 멤버와이즈 이니셜라이저에 대한 거대한 향상

**Massive improvements to synthesized memberwise initializers**

SE-0242는 Swift에서 가장 흔하게 사용되는 특징 중 하나인, 구조체에 대한 멤버와이즈 이니셜라이저에 대한 주요 향상을 도입합니다.

Swift의 초기 버전에서 멤버와이즈 이니셜라이저는 구조체의 프로퍼티와 일치하는 매개변수를 받기 위해 자동으로 생성되었습니다.

```swift
struct User {
  var name: String
  var loginCount: Int = 0
}

let piper = User(name: "Piper Chapman", loginCount: 0)
```

Swift 5.1에서 이는 이제 멤버와이즈 이니셜라이저가 기본값을 갖는 어떠한 프로퍼티에 대하여 기본 매개변수 값을 사용할 수 있도록 강화되었습니다. `User` 구조체에서 우리는 `loginCount`에 기본값 0을 주었으며, 이는 멤버와이즈 이니셜라이저에서 이것을 명시할 수도 있고 빠뜨릴 수도 있다는 것을 의미합니다.

```swift
let gloria = User(name: "Gloria Mendoza", loginCount: 0)
let suzanne = User(name: "Suzanne Warren")
```

이는 코드의 중복을 방지하며, 우리가 항상 반기는 것입니다.

## 단일 표현식을 갖는 함수의 암시적 반환

**Implicit returns from single-expression functions**

SE-0255는 언어에서 작지만 중요한 모순적인 것을 제거했습니다. 값을 반환하는 단일 표현식은 이제 `return` 키워드를 제거할 수 있으며 Swift는 암시적으로 이를 이해할 것입니다.

Swift의 이전 버전에서 값을 반환하는 한 줄의 클로저에서 `return` 키워드를 생략할 수 있었습니다. 코드가 오직 한 줄만 있다는 것은 이것이 반드시 값을 반환하는 코드라는 것을 의미하기 때문입니다. 그러므로 다음의 두 코드 조각은 동일했습니다.

```swift
let doubled1 = [1, 2, 3].map { $0 * 2 }
let doubled2 = [1, 2, 3].map { return $0 * 2 }
```

Swift 5.1에서, 이 행위는 이제 함수에도 확장되었습니다. 함수가 효과적으로 값을 평가하는 단일 코드 조각, 단일 표현식을 포함한다면 `return` 키워드를 생략할 수 있습니다.

```swift
func double(_ number: Int) -> Int {
  number * 2
}
```

이는 아마도 어떤 사람들에게는 처음에는 두 배의 것을 취할 것이지만, 나는 그것이 시간이 지남에 따라 제2의 천성이 될 것임을 확신합니다.

## 범용 Self

**Universal Self**

SE-0068은 Swift의 `Self`가 클래스, 구조체, 열거형 내에서 사용될 때 포함하는 타입을 참조하도록 확장합니다. 이는 특히 동적 타입에서 유용합니다. 이는 어떤 것의 정확한 타입이 런타임에서 결정될 필요가 있는 것이빈다.

```swift
class NetworkManager {
  class var maximumActiveRequests: Int {
    return 4
  }

  func printDebugData() {
    print("Maximum network requests: \(NetworkManager.maximumActiveRequests).")
  }
}

```

이는 네트워크 매니저에서 정적 `maximumActiveRequests` 프로퍼티를 선언하고 정적 프로퍼티를 출력하기 위해 `printDebugData()` 메소드를 추가합니다. 이는 곧바로 잘 동작하지만, `NetworkManager`가 서브클래싱될 때 이것은 더 복잡해집니다.

```swift
class ThrottledNetworkManager: NetworkManager {
  override class var maximumActiveRequests: Int {
    return 1
  }
}
```

이 서브클래스는 `maximumActiveRequests`를 변경하여 한 번에 오직 하나의 요청만을 허용하도록 합니다. 하지만 `printDebugData()`를 호출한다면 부모 클래스에 있는 값을 출력할 것입니다.

```swift
let manager = ThrottledNetworkManager()
manager.printDebugData()
```

이는 4 대신 1을 출력해야 할 것이며, 이것이 SE-0068이 초래하는 것입니다. 우리는 이제 현재 타입을 참조하기 위해 `Self`를 작성할 수 있습니다. 그러므로 `printDebugData()`를 다음과 같이 다시 작성할 수 있습니다.

```swift
class ImprovedNetworkManager {
  class var maximumActiveRequests: Int {
    return 4
  }

  func printDebugData() {
    print("Maximum network requests: \(Self.maximumActiveRequests).")
  }
}
```

이는 `Self`가 Swift 초기 버전의 프로토콜에서의 동작과 같은 방식으로 동작한다는 것을 의미합니다.

## 불투명 반환 타입

**Opaque return types**

SE-0244는 Swift에 불투명 타입*opaque types*의 개념을 도입합니다. 불투명 타입은 그 객체의 종류가 무엇인지 명확하게 알지 않고 객체의 능력에 대해 말할 수 있는 것입니다.

처음 봤을 때 이는 프로토콜과 같이 들리지만, 불투명 타입은 프로토콜과 현저하게 다른데, 프로토콜은 연관 타입과 함께 동작할 수 있고, 매 시간 내부적으로 사용되는 동일한 타입을 필요로 하며, 상세 구현을 숨기는 것을 허용하기 때문입니다.

예를 들어, Rebel 베이스로부터 다른 종류의 파이터를 출전시키기 원한다면 다음과 같은 코드를 작성할 수 있을 것입니다.

```swift
protocol Fighter { }
struct XWing: Fighter { }

func launchFighter() -> Fighter {
  return XWing()
}

let red5 = launchFighter()
```

이 함수를 호출한 것은 이것이 어떠한 종류의 `Fighter`를 반환할 것임을 알지만 정확하게 그것이 무엇인지 알지 못합니다. 결과적으로, 우리는 `struct YWing: Fighter { }`나 다른 타입에 추가할 수 있을 것이고, 그것들 중 어떠한 것이라도 반환될 수 있습니다.

하지만 문제가 있습니다. 특정 파이터가 Red 5임을 확인하기 원한다면? 당신은 아마 `Fighter`가 `Equatable` 프로토콜을 준수하게 하여 `==`를 사용할 수 있게 하는 것이 해결책이라고 생각할 수 있습니다. 하지만, 그렇게 하자마자 Swift는 `launchFighter` 함수에 대하여 현저하게 두려운 에러를 던질 것입니다. "프로토콜 'Fighter'는 Self나 연관 타입을 요구하기 때문에 제네릭 제약으로만 사용되어야 합니다."

"Self"에 관한 에러가 이와 관련된 것입니다. `Equatable` 프로토콜은 그것들이 같은지 확인하기 위해 두 개의 인스턴스 그 자체("Self")를 비교해야 합니다. 하지만 Swift는 두 개의 동일시할 수 있는 것이 간접적으로 동일하다는 것을 보장하지 않습니다. 예를 들어 우리는 Fighter를 정수 배열과 비교할 수 있을 것입니다.

불투명 타입은 이 문제를 해결합니다. 우리가 방금 프로토콜이 사용되는 것을 확인했을지라도, Swift 컴파일러는 그 프로토콜이 정확히 해결하는 것이 무엇인지 알기 때문입니다. 그것이 `XWing`인지, 문자열 배열인지, 또는 어떠한 것이든지 말입니다.

불투명 타입을 돌려주기 위해 프로토콜 이름 이전에 `some` 키워드를 사용하십시오.

```swift
func launchOpaqueFighter() -> some Fighter {
  return XWing()
}
```

호출자의 관점에서 이것은 여전히 `Fighter`를 돌려줄 것이며, 이는 `XWing`도, `YWing`도, 또는 `Fighter` 프로토콜을 준수하는 어떠한 것이라도 될 수 있을 것입니다. 하지만 컴파일러의 관점에서는 이것이 반환할 것을 정확히 알고 있으므로, 모든 규칙을 올바르게 준수할 것임을 보장할 수 있습니다.

예를 들어, 이처럼 `some Equatable`을 반환하는 함수를 고려해 봅시다.

```swift
func makeInt() -> some Equatable {
  Int.random(in: 1...10)
}
```

우리가 이것을 호출할 때, 우리 모두는 이것이 `Equatable` 한 어떠한 종류의 값임을 알고 있습니다. 그러나 이것을 두 번 호출한다면 우리는 호출들의 결과를 비교할 수 있습니다. Swift는 그것이 동일한 기반 타입임을 확신하기 때문입니다.

```swift
let int1 = makeInt()
let int2 = makeInt()
print(int1 == int2)
```

이처럼 `some Equatable`을 반환하는 두 번째 함수가 있다면 동일함은 참이 아닐 것입니다.

```swift
func makeString() -> some Equatable {
  "Red"
}
```

우리의 관점에서 두 개의 것이 `Equatable` 타입을 반환하고, `makeString()`의 두 번의 호출이나 `makeInt()`의 두 번의 호출의 결과를 비교할 수 있을지라도, Swift는 `makeString()`의 반환값과 `makeInt()`의 반환값을 비교하는 것을 못하게 합니다. Swift는 문자열과 정수를 비교하는 것이 말이 되지 않는다는 것을 알고 있기 때문입니다.

여기에서의 중요한 단서는 불투명 반환 타입을 갖는 함수는 항상 어떠한 특정 타입을 반환해야 한다는 것입니다. 예를 들어 `Bool.random()`을 사용하여 `XWing`이나 `YWing`을 임의로 출전시키려 한다면, Swift는 우리의 코드를 빌드하는 것을 거절할 것입니다. 컴파일러는 어떤 것을 반환할지 더이상 말할 수 없기 때문입니다.

당신은 아마도 "항상 같은 타입을 반환할 필요가 있다면, 왜 `func launchFighter() -> XWing`과 같은 코드를 작성하지 않는가?"라고 생각할 수 있을 것입니다. 때때로 이는 잘 동작하지만 다음과 같은 새로운 문제를 만들어 냅니다.

- 세계에 드러내기 원하지 않는 타입으로 끝납니다. 예를 들어 `someArray.lazy.drop { … }`과 같은 코드를 사용한다면, `LazyDropWhileSequence`라는, Swift 표준 라이브러리의 헌신적이고 매우 명확한 타입을 반환받습니다. 우리가 정말로 신경쓰는 모든 것은 이것이 시퀀스라는 것입니다. 우리는 Swift가 내부적으로 작동하는 방법을 알 필요가 없습니다.
- 이후에 생각을 바꾸기 위한 능력을 잃어버립니다. `launchFighter()`가 오직 `XWing`만을 반환하게 하는 것은 미래에 다른 타입으로 바꿀 수 없다는 것을 의미합니다. 디즈니가 스타 워즈 장난감 판매에 의존하는 것이 얼마나 큰 문제가 되는지 생각해 보십시오! 불투명 타입을 반환하게 하여 오늘날에는 X-Wing을 반환하도록, 일년 후에는 B-Wing을 반환하도록 할 수 있습니다. 우리는 코드에 주어진 빌드에 있는 것을 반환할 뿐이지만, 이후에 생각의 변화에 대한 유연성을 여전히 취할 수 있습니다.

어떠한 점에서 이는 제네릭과 비슷하게 들릴 수 있습니다. 이는 또한 "Self나 연관 타입 요구" 문제도 해결해줍니다. 제네릭 코드는 다음과 같은 코드를 작성하게 해줍니다.

```swift
protocol ImperialFighter {
  init()
}

struct TIEFighter: ImperialFighter { }
struct TIEAdvanced: ImperialFighter { }

func launchImperialFighter<T: ImperialFighter>() -> T {
  return T()
}
```

새로운 프로토콜을 정의하였고, 이를 준수하는 타입은 매개변수가 없이 이니셜라이징 가능한 것을 요구합니다. 그리고 이 프로토콜을 준수하는 두 개의 구조체를 정의합니다. 그리고 이를 사용하기 위해 제네릭 함수를 생성합니다. 그러나 여기서의 차이점은 이제 `launchImperialFighter()`의 호출자가 그것이 취할 파이터의 종류를 선택한다는 것입니다.

```swift
let fighter1: TIEFighter = launchImperialFighter()
let fighter2: TIEAdvanced = launchImperialFighter()
```

호출자가 그들의 데이터 타입을 선택할 수 있게 한다면 제네릭은 잘 동작할 것입니다. 하지만 그 함수가 반환 타입을 결정하게 하기를 원한다면 그것은 실패할 것입니다.

그러므로, 불투명 타입은 다음과 같은 것들을 허용합니다.

- 함수는 어떠한 종류의 타입을 반환할지 결정하며, 그 함수의 호출자가 결정하는 것이 아닙니다.
- Self나 연관 타입 요구에 대해 걱정하지 않아도 됩니다. 컴파일러는 정확하게 그 타입이 무엇인지 알고 있기 때문입니다.
- 필요할 때마다 우리의 생각을 바꿀 수 있습니다.
- 외부 세계에 private, internal 타입을 드러내지 않습니다.

## 정적 및 클래스 서브스크립트

**Static and class subscripts**

SE-0254는 서브스크립트가 정적이게 되는 능력을 추가합니다. 이는 타입의 인스턴스 대신 타입에 적용된다는 것을 의미합니다.

정적 프로퍼티 및 메소드는 어떠한 값의 집합이 그 타입의 모든 인스턴스에서 공유될 때 사용됩니다. 예를 들어 앱의 설정을 저장하는 중앙화된 타입을 가지고 있다면 다음과 같은 타입을 작성할 수 있을 것입니다.

```swift
public enum OldSettings {
  private static var values = [String: String]()
  
  static func get(_ name: String) -> String? {
    return values[name]
  }
  
  static func set(_ name: String, to newValue: String?) {
    print("Adjusting \(name) to \(newValue ?? "nil")")
    values[name] = newValue
  }
}

OldSettings.set("Captain", to: "Gary")
OldSettings.set("Friend", to: "Mooncake")
print(OldSettings.get("Captain") ?? "Unknown")
```

타입 내부에 딕셔너리를 감싸는 것은 더욱 조심스럽게 접근을 제어할 수 있다는 것을 의미합니다. 또한 케이스가 없는 열거형을 사용하는 것은 타입을 인스턴스화하려는 시도를 할 수 없다는 것을 의미합니다. 우리는 `Settings`의 다양한 인스턴스를 만들 수 없습니다.

Swift 5.1에서 우리는 대신 정적 서브스크립트를 사용할 수 있으며, 다음과 같이 코드를 다시 작성할 수 있습니다.

```swift
public enum NewSettings {
  private static var values = [String: String]()
  
  public static subscript(_ name: String) -> String? {
    get {
      return values[name]
    }
    set {
      print("Adjusting \(name) to \(newValue ?? "nil")")
      values[name] = newValue
    }
  }
}

NewSettings["Captain"] = "Gary"
NewSettings["Friend"] = "Mooncake"
print(NewSettings["Captain"] ?? "Unknown")
```

이와 같은 사용자 정의 서브스크립트는 타입의 인스턴스에서 가능했습니다. 이 향상은 정적 또는 클래스 서브스크립트에서도 가능하게 합니다.

## 모호한 none 케이스에 대한 경고

**Warnings for ambiguous none cases**

Swift의 옵셔널은 두 개의 케이스 `some`과 `none`을 갖는 열거형으로 구현되어 있습니다. 이는 우리 자신의 열거형이 `none` 케이스를 갖고 있고 옵셔널로 감싸져 있을 때 혼란의 가능성을 만들어냅니다.

```swift
enum BorderStyle {
  case none
  case solid(thickness: Int)
}
```

옵셔널이 아닐 때 사용하는 것은 항상 명백합니다.

```swift
let border1: BorderStyle = .none
print(border1)
```

이는 "none"을 출력할 것입니다. 하지만 어떠한 경계선 스타일을 사용할지 몰라서 이 열거형에 옵셔널을 사용한다면 문제에 맞닥뜨리게 됩니다.

```swift
let border2: BorderStyle? = .none
print(border2)
```

이는 "nil"을 출력할 것입니다. Swift는 `.none`이 `BorderStyle.none`을 갖는 옵셔널이라고 생각하는 대신, 이 옵셔널이 비어 있음을 의미한다고 가정하기 때문입니다.

Swift 5.1에서 이제 이 혼란은 경고를 출력합니다. "'Optional.none'을 의미한다고 가정합니다. 대신 'BorderStyle.none'을 의미합니까?" 이는 에러의 소스 호환성이 손상되는 것을 피할 수 있지만, 적어도 개발자에게 그들의 코드가 그들이 생각한 것을 의미하지 않는다는 것을 알려줍니다.

## 옵셔널이 아닌 열거형과 옵셔널인 열거형 간의 매칭

**Matching optional enums against non-optionals**

Swift는 문자열과 정수에 대하여 옵셔널 및 옵셔널이 아닌 것 사이에 switch/case 패턴 매칭을 처리할 수 있을 만큼 영리했습니다. 하지만 Swift 5.1 이전에 이는 열거형에 확장되어 있지 않았습니다.

Swift 5.1에서 우리는 이제 옵셔널이 아닌 열거형과 옵셔널인 열거형에 switch/case 패턴 매칭을 사용할 수 있습니다.

```swift
enum BuildStatus {
  case starting
  case inProgress
  case complete
}

let status: BuildStatus? = .inProgress

switch status {
case .inProgress:
  print("Build is starting…")
case .complete:
  print("Build is complete!")
default:
  print("Some other build status")
}
```

Swift는 옵셔널이 아닌 케이스에 옵셔널인 열거형을 직접적으로 비교할 수 있습니다. 그러므로 이 코드는 "Build is starting…"을 출력할 것입니다.

## 순서 있는 컬렉션의 차이

**Ordered collection diffing**

SE-0240은 순서 있는 컬렉션 사이의 차이를 계산하고 적용하기 위한 능력을 도입합니다. 이는 테이블 뷰에 있는 복잡한 컬렉션을 갖는 개발자에게 특히 흥미로운 것일 수 있습니다. 그들은 애니메이션을 사용하여 매끄럽게 많은 아이템을 추가하고 삭제하기 원합니다.

기본 원리는 확실합니다. Swift 5.1은 새로운 `difference(from:)` 메소드를 제공하여 두 개의 순서 있는 컬렉션 간의 차이를 계산하여, 어떠한 아이템이 삭제되고 어떠한 아이템이 삽입되는지를 계산합니다. 이는 `Equatable` 요소를 포함하는 어떠한 순서 있는 컬렉션에라도 사용될 수 있습니다.

이를 보이기 위해, 우리는 점수 배열을 생성하고, 어떤 것과 다른 것의 차이를 계산하고, 이 차이에 대하여 반복하고 각각을 적용하여 두 개의 컬렉션을 동일하게 만들 수 있습니다.

**알아두기** Swift는 이제 Apple의 운영체제 내에 실리게 되므로, 이와 같은 새로운 기능은 해당 코드가 새로운 기능을 포함하는 운영체제에서 동작하는지 확인하기 위해 `#available`을 사용해야 합니다. 미래의 어느 시점에 알려지지 않은, 발표되지 않은 운영체제에 탑재될 기능의 경우, "9999"라는 특별한 버전 번호는 "아직 실제 숫자가 무엇인지 알 수 없다"는 의미로 사용됩니다.

```swift
var scores1 = [100, 91, 95, 98, 100]
let scores2 = [100, 98, 95, 91, 100]

if #available(iOS 9999, *) {
  let diff = scores2.difference(from: scores1)
  for change in diff {
    switch change {
      case .remove(let offset, _, _):
        scores1.remove(at: offset)
      case .insert(let offset, let element, _):
        scores1.insert(element, at: offset)
    }
  }
  print(scores1)
}
```

더 진보된 애니메이션을 위해, 변화에 대한 세 번째 값 `associatedWith`를 사용할 수 있습니다. 그러므로, `.insert(let offset, let element, __)` 대신 `.insert(let offset, let element, let associatedWith)`를 사용할 수 있습니다. 이는 같은 시간에 일어난 변화의 쌍을 추적할 수 있게 합니다. 컬렉션에서 아이템을 두 지점 아래로 옮긴 것은 제거 후 삽입이지만, `associatedWith` 값은 이러한 두 개의 변화를 함께 묶어 이를 이동으로 처리할 수 있게 합니다.

직접 이러한 변화를 적용하는 대신, `applying()` 메소드를 사용하여 컬렉션 전체에 적용할 수 있습니다.

```swift
if #available(iOS 9999, *) {
  let diff = scores2.difference(from: scores1)
  let result = scores1.applying(diff) ?? []
}
```

## 이니셜라이징되지 않은 배열 생성

**Creating uninitialized arrays**

SE-0245는 기본적으로 값이 채워지지 않은 배열에 대한 새로운 이니셜라이저를 도입합니다. 이는 이전에는 private API로 사용 가능했으며, 이는 Xcode가 코드 자동 완성 리스트에 소개하지는 않지만 원한다면 여전히 사용 가능했다는 것을 의미합니다. 또한 미래에 그것이 없어지지 않을 것이라는 위험을 떠맡을 수 있었음을 의미합니다.

그 이니셜라이저를 사용하기 위해 원하는 용량을 말하고 필요하다면 값을 채우기 위한 클로저를 제공합니다. 클로저에는 값을 쓸 수 있는 안전하지 않은 가변 버퍼 포인터, 실제로 사용된 값의 개수를 다시 보고할 수 있는 inout 두 번째 매개변수가 제공될 것입니다.

예를 들어 이처럼 10개의 임의의 정수를 갖는 배열을 만들 수 있을 것입니다.

```swift
let randomNumbers = Array<Int>(unsafeUninitializedCapacity: 10) { buffer, initializedCount in 
  for x in 0..<10 {
    buffer[x] = Int.random(in: 0...10)
  }
  initializedCount = 10
}
```

여기에는 몇 가지 규칙이 있습니다.

1. 당신은 당신이 요청한 모든 용량을 사용할 필요가 없습니다. 하지만 그 용량을 넘어서서는 안됩니다. 그러므로, 당신이 10의 용량을 요청했다면 `initializedCount`는 0에서 10까지 설정 가능하지만 11은 안됩니다.
2. 배열 안에 들어갈 요소를 이니셜라이즈하지 않았다면, 예를 들어 `initializedCount`를 5로 설정했지만 실제로 0에서 4까지의 요소에 대한 값만을 제공했다면, 임의의 데이터로 채워질 것입니다. 이는 좋지 않습니다.
3. `initializedCount`를 설정하지 않는다면 0이 될 것입니다. 그러므로 할당한 어떠한 데이터라도 손실될 것입니다.

이제 위의 코드를 `map()`을 사용하여 다시 작성할 수 있습니다.

```swift
let randomNumbers2 = (0...9).map { _ in Int.random(in: 0...10) }
```

확실히 이것이 더 읽기 쉬우나 덜 효율적입니다. 이것은 범위를 만들고, 새로운 빈 배열을 만들고, 올바른 양만큼 사이즈를 키우고, 배열을 순환하고, 각각의 범위 아이템마다 클로저를 호출합니다.