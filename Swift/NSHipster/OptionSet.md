# OptionSet

Objective-C는 *option* 타입 또는 함께 조합될 수 있는 값들의 집합을 정의하기 위해 `NS_OPTIONS` 매크로를 사용합니다. 예를 들어, UIKit의 `UIViewAutoresizing` 타입에 있는 값들은 비트와이즈 OR 연산자 (`|`) 를 사용하여 조합되어 어떤 마진과 크기가 자동으로 리사이징되는지 지정하기 위해 `UIView` 의 `autoresizingMask` 프로퍼티에 전달될 수 있습니다.

```objective-c
typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
    UIViewAutoresizingNone                 = 0,
    UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
    UIViewAutoresizingFlexibleWidth        = 1 << 1,
    UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
    UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
    UIViewAutoresizingFlexibleHeight       = 1 << 4,
    UIViewAutoresizingFlexibleBottomMargin = 1 << 5
};
```

Swift는 이것과 다른 `NS_OPTIONS` 매크로를 사용하여 정의된 다른 타입들을 `OptionSet` 프로토토콜을 준수하는 구조체로 가져옵니다.

```swift
extension UIView {
    struct AutoresizingMask: OptionSet {
        init(rawValue: UInt)

        static var flexibleLeftMargin: UIView.AutoresizingMask
        static var flexibleWidth: UIView.AutoresizingMask
        static var flexibleRightMargin: UIView.AutoresizingMask
        static var flexibleTopMargin: UIView.AutoresizingMask
        static var flexibleHeight: UIView.AutoresizingMask
        static var flexibleBottomMargin: UIView.AutoresizingMask
    }
}
```

> 가져온 타입의 이름을 다시 짓는 것과 타입 중첩은 개별 매커니즘의 결과입니다.

(이전에 `RawOptionSetType` 이었던) `OptionSet` 이 도입된 시점에서 이는 언어가 제공할 수 있는 최상의 캡슐화 방식이었습니다. 이 글의 마지막까지 우리는 `OptionSet` 의 기능을 향상시키기 위해 Swift 4.2에 추가된 언어의 특징의 이점을 취하는 방법을 보일 것입니다.

...하지만 그것은 우리를 앞서가고 있습니다.

이번 주에는 import된 `OptionSet` 타입을 사용하는 것과, 어떻게 당신만의 것을 만들 수 있는지 찬찬히 살펴 봅시다. 이후에 우리는 옵션 설정에 대하여 다른 선택지를 제공할 것입니다.

## Working with Imported Option Set Types

문서에 따르면, Apple SDK에는 `ARHitTestResult.Resultype` 부터 `XMLNode.Options` 에 이르기까지 300개가 넘는 타입이 `OptionSet` 을 준수하고 있습니다.

어떤 것을 가지고 작업하든지간에, 그것들을 사용하는 방법은 항상 같습니다.

옵션을 하나 지정하기 위해 그것을 직접 넘겨줍니다. (Swift는 프로퍼티를 설정할 때 타입을 추론할 수 있으므로 점을 찍기까지의 모든 것을 생략할 수 있습니다.)

```swift
view.autoresizingMask = .flexibleHeight
```

`OptionSet` 은 `SetAlgebra` 프로토콜을 채택하므로, 이 때문에 비트와이즈 작업 없이 배열 리터럴을 사용하여 여러 개의 옵션을 지정할 수 있습니다.

```swift
view.autoresizingMask = [.flexibleHeight, .flexibleWidth]
```

옵션을 지정하지 않으려면 빈 배열 리터럴 (`[]`) 을 넘겨주세요.

```swift
view.autoresizingMask = [] // 옵션 없음
```

## Declaring Your Own Option Set Types

닫힌 값 집합으로부터의 조합을 저장하는 프로퍼티를 가지고 있고 그 조합이 비트셋을 사용하여 효율적으로 저장되기를 원한다면 당신만의 옵션 셋 타입을 만드는 것을 고려할 수 있을 것입니다.

이를 위해 `OptionSet` 프로토콜을 채택하는 새로운 구조체를 선언하는데, 이는 `rawValue` 인스턴스 프로퍼티와, 당신이 나타내고 싶어하는 각각의 값에 대한 타입 프로퍼티를 요구합니다. 이것들의 원시 값은 2의 거듭 제곱으로 증가하여 초기화되는데, 이는 증가하는 오른쪽 값에 왼쪽 비트 쉬프트 (`<<`) 작업을 사용하여 만들어질 수 있습니다. 또한 특정 값 조합에 대하여 별칭을 지정할 수 있습니다.

예를 들어 다음은 피자의 토핑 옵션을 나타내는 방법을 보여줍니다.

```swift
struct Toppings: OptionSet {
    let rawValue: Int

    static let pepperoni    = Toppings(rawValue: 1 << 0)
    static let onions       = Toppings(rawValue: 1 << 1)
    static let bacon        = Toppings(rawValue: 1 << 2)
    static let extraCheese  = Toppings(rawValue: 1 << 3)
    static let greenPeppers = Toppings(rawValue: 1 << 4)
    static let pineapple    = Toppings(rawValue: 1 << 5)

    static let meatLovers: Toppings = [.pepperoni, .bacon]
    static let hawaiian: Toppings = [.pineapple, .bacon]
    static let all: Toppings = [
        .pepperoni, .onions, .bacon,
        .extraCheese, .greenPeppers, .pineapple
    ]
}
```

이 문맥에 맞는 더 큰 예제입니다.

```swift
struct Pizza {
    enum Style {
        case neapolitan, sicilian, newHaven, deepDish
    }

    struct Toppings: OptionSet { ... }

    let diameter: Int
    let style: Style
    let toppings: Toppings

    init(inchesInDiameter diameter: Int,
         style: Style,
         toppings: Toppings = [])
    {
        self.diameter = diameter
        self.style = style
        self.toppings = toppings
    }
}

let dinner = Pizza(inchesInDiameter: 12,
                   style: .neapolitan,
                   toppings: [.greenPeppers, .pineapple])
```

`OptionSet` 이 `SetAlgebra` 를 준수하여 얻는 또다른 이점은 멤버십 결정, 요소 삽입 및 제거, 합집합 및 교집합과 같은 집합 연산을 수행할 수 있다는 것입니다. 예를 들어 이는 어떤 피자가 채식주의자에게 알맞는지를 결정하기 쉽게 합니다.

```swift
extension Pizza {
    var isVegetarian: Bool {
        return toppings.isDisjoint(with: [.pepperoni, .bacon])
    }
}

dinner.isVegetarian // true
```

## A Fresh Take on an Old Classic

좋습니다. 이제 `OptionSet` 을 어떻게 사용하는지 알았습니다. `OptionSEt` 을 사용하지 않는 방법을 보여주겠습니다.

이전에 언급한 것처럼, Swift 4.2의 새로운 언어의 특징이 ~~케이크~~피자 파이를 만들고 먹을 수도 있게 해줍니다.

먼저 `RawRepresentable`, `Hashable`, `CaseIterable` 을 상속받는 새로운 `Option` 프로토콜을 선언합니다.

```swift
protocol Option: RawRepresentable, Hashable, CaseIterable { }
```

다음으로 `String` 원시 값을 갖고 `Option` 프로토콜을 채택하는 열거형을 선언합니다.

```swift
enum Topping: String, Option {
  case pepperoni, onions, becon,
       extraCheese, greenPeppers, pineapple
}
```

이전의 구조체 선언과 이 열거형을 비교해 봅시다. 훨씬 낫지 않나요? 기다려 보세요. 훨씬 더 나아질 것입니다.

`Hashable` 의 자동 통합*automatic synthesis*은 `Set` 을 사용하는 것을 쉽게 해주는데, 이것으로 `OptionSet` 의 기능을 절충할 수 있습니다. 조건적 준수*conditional conformance*를 사용하여 요소가 `Topping` 인 `Set` 에 대한 익스텐션을 만들고 이름 있는 토핑 콤보를 정의할 수 있습니다. 보너스로 `CaseIterable` 은 *"작품the works"*으로 피자를 주문하게 쉽게 해줍니다.

```swift
extension Set where Element == Topping {
    static var meatLovers: Set<Topping> {
        return [.pepperoni, .bacon]
    }

    static var hawaiian: Set<Topping> {
        return [.pineapple, .bacon]
    }

    static var all: Set<Topping> {
        return Set(Element.allCases)
    }
}

typealias Toppings = Set<Topping>
```

`CaseIterable` 로 할 수 있는 것을 아직 다 하지 않았습니다. `allCases` 타입 프로퍼티를 열거*enumerating*하여 각각의 케이스에 대하여 비트셋 값을 자동으로 생성할 수 있는데, 이는 `Option` 타입을 포함하는 `Set` 에 대하여 동등한 `rawValue` 를 만들기 위해 조합할 수 있습니다.

```swift
extension Set where Element: Option {
    var rawValue: Int {
        var rawValue = 0
        for (index, element) in Element.allCases.enumerated() {
            if self.contains(element) {
                rawValue |= (1 << index)
            }
        }

        return rawValue
    }
}
```

`OptionSet` 과 `Set` 모두가 `SetAlgebra` 를 준수하기 때문에 우리의 새로운 `Topping` 구현은 `Pizza` 자체에 대하여 어떠한 것도 변경될 필요 없이 기존의 것과 교체될 수 있습니다.

> 이 접근법은 `CaseIterable` 이 제공하는 케이스의 순서가 안정되어 있다고 가정합니다. 그렇지 않다면 옵션의 조합을 위한 생성된 원시 값은 일관되지 않을 수 있습니다.

요약해 봅시다. 당신은 Swift로 Apple SDK를 사용하여 작업할 때 `OptionSet` 과 마주치게 될 것입니다. 그리고 당신이 `OptionSet` 을 준수하는 당신만의 구조체를 만들 수 있을지라도, 아마 그럴 필요가 없을 것입니다. 이 글의 끝에 설명한 요령을 사용할 수도 있고, 더 직관적인 방법을 사용할 수 도있습니다.

Whichever option you choose, you should be all set.