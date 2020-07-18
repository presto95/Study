# 타입

Swift에는 두 가지 종류의 타입이 있다: 이름 있는 타입과 합성 타입이다. 이름 있는 타입*named type*은 정의될 때 주어진 특정한 이름이 될 수 있는 타입이다. 이름 있는 타입은 클래스, 구조체, 열거형, 프로토콜을 포함한다. 예를 들어, 사용자가 정의한 `MyClass`라는 이름을 가즌 클래스의 인스턴스는 `MyClass` 타입을 갖는다. 사용자가 정의한 이름 있는 타입 뿐만 아니라, Swift 표준 라이브러리는 일반적으로 사용되는 많은 이름 있는 타입을 정의한다. 이는 배열, 딕셔너리, 그리고 옵셔널 값을 나타내는 것들을 포함한다.

다른 언어에서 일반적으로 기본적 또는 원시적으로 간주되는 데이터 타입-숫자, 문자, 문자열을 표헌하는 타입 같은 것들-은 실제로 이름 있는 타입이며, 구조체를 사용하여 Swift 표준 라이브러리에 정의되고 구현되었다. 이들이 이름 있는 타입이기 때문에, 익스텐션 선언을 사용하여 당신의 프로그램의 요구에 맞는 동작을 확장할 수 있다.

합성 타입*compound type*은 이름이 없이 Swift 언어 자체에서 정의된 타입이다. 두 가지 합성 타입이 있다: 함수 타입과 튜플 타입이다. 합성 타입은 이름 있는 타입과 다른 합성 타입을 포함할 수 있다. 예를 들어, 튜플 타입 `(Int, (Int, Int))`는 두 가지 요소를 포함한다: 첫 번째는 이름 있는 타입 `Int`이고, 두 번째는 또다른 혼합 타입 `(Int, Int)`이다.

이름 있는 타입이나 합성 타입 주변에 소괄호를 둘 수 있다. 그러나, 타입 주변에 소괄호를 두는 것은 어떠한 효과를 주지 않는다. 예를 들어, `(Int)`는 `Int`와 동일하다.

이 챕터는 Swift 언어 자체가 정의한 타입에 대해 논의하고 Swift의 타입 추론 동작을 기술한다.

## 타입 어노테이션

*타입 어노테이션*은 명시적으로 변수나 표현식의 타입을 지정한다. 타입 어노테이션은 콜론 (`:`) 으로 시작하고 타입으로 끝난다.

```swift
let someTuple: (Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int) { /* ... */ }
```

첫 번째 예제에서, 표현식 `someTuple`은 튜플 타입 `(Double, Double)`을 갖도록 지정되었다. 두 번째 예제에서, 함수 `someFunction`에 대한 매개변수 `a`는 `Int` 타입을 갖도록 지정되었다.

타입 어노테이션은 타입 이전에 타입 애트리뷰트 리스트를 선택적으로 포함할 수 있다.

## 타입 식별자

*타입 식별자*는 이름 있는 타입이나 이름 있는 타입 또는 혼합 타입의 타입 별칭을 참조한다.

대부분의 타입 식별자는 식별자의 이름과 같은 이름 있는 타입에 직접 참조한다. 예를 들어, `Int`는 이름 있는 타입 `Int`를 직접 참조하는 타입 식별자고, `Dictionary<String, Int>` 타입 식별자는 이름 있는 타입 `Dictionary<String, Int>`를 직접 참조한다.

타입 식별자가 같은 이름을 가진 타입을 참조하지 않는 두 가지 경우가 있다. 첫 번째 경우에, 타입 식별자는 이름 있는 타입 또는 합성 타입의 타입 별칭을 참조한다. 예를 들어, 아래의 예제에서, 타입 어노테이션에서 `Point`의 사용은 튜플 타입 `(Int, Int)`를 참조한다.

```swift
typealias Point = (Int, Int)
let origin: Point = (0, 0)
```

두 번째 경우에, 타입 식별자는 다른 모듈에 선언된 이름 있는 타입이나 다른 타입 안에 있는 중첩 타입을 참조하기 위해 점 (`.`) 신택스를 사용한다. 예를 들어, 다음의 코드에서 타입 식별자는 `ExampleModule` 모듈에 선언된 이름 있는 타입 `MyType`을 참조한다.

```swift
var someValue: ExampleModule.MyType
```

## 튜플 타입

*튜플 타입*은 콤마로 구분된 타입의 리스트이며, 소괄호로 둘러싸여 있다.

함수가 여러 개의 값을 포함하는 하나의 튜플을 반환할 수 있도록 함수의 반환형을 튜플 타입으로 사용할 수 있다. 또한 튜팔 타입의 요소에 이름을 지을 수 있고 각각의 요소 값을 참조하기 위해 그러한 이름들을 사용할 수 있다. 요소 이름은 콜론 (`:`) 바로 뒤에 이어지는 식별자로 구성된다.

튜플 타입의 요소가 이름을 가진다면, 그 이름은 타입의 일부분이 된다.

```swift
var someTuple = (top: 10, bottom: 12)  // someTuple is of type (top: Int, bottom: Int)
someTuple = (top: 4, bottom: 42) // OK: names match
someTuple = (9, 99)              // OK: names are inferred
someTuple = (left: 5, right: 5)  // Error: names don't match
```

모든 튜플 타입은 빈 튜플 타입 `()`의 타입 별칭인 `Void`를 제외하고, 두 개 이상의 타입을 포함한다.

## 함수 타입

*함수 타입*은 함수, 메소드 또는 클로저의 타입을 나타내고 화살표 (`→`)로 구분된 매개변수와 반환형으로 구성된다.

```swift
({parameter type}) -> {return type}
```

*매개변수 타입*은 콤마로 구분된 타입의 리스트이다. *반환형*은 튜플 타입이 될 수 있기 때문에, 함수 타입은 여러 개의 값을 반환하는 함수와 메소드를 지원한다.

함수 타입 `() -> T` (`T`가 어떠한 타입이든지) 의 매개변수는 호출 위치에서 암시적으로 클로저를 만들기 위해 `autoclosure` 애트리뷰트를 적용할 수 있다. 이는 함수를 호출할 때 명시적으로 클로저를 작성할 필요 없이 표현식을 평가하는 것을 미루기 위해 구문적으로 편리한 방법을 제공한다.

함수 타입은 그것의 *매개변수 타입* 안에 가변 매개변수를 가질 수 있다. 구문적으로, 가변 매개변수는 `Int...` 처럼 기반 타입 바로 뒤에 세 개의 점 (`...`) 이 붙는 것으로 구성된다. 가변 매개변수는 기반 타입 이름의 요소들을 포함하는 배열로 취급된다. 예를 들어, 가변 매개변수 `Int...`는 `[Int]`로 취급된다.

인-아웃 매개변수를 지정하려면 `inout` 키워드를 매개변수 타입 앞에 붙여라. 가변 매개변수나 반환형을 `inout` 키워드와 함께 표시할 수 없다.

함수 타입이 오직 하나의 매개변수를 가지고 있고, 그 매개변수의 타입이 튜플 타입이라면, 그 튜플 타입은 함수의 타입을 작성할 때 반드시 소괄호로 둘러싸여 있어야 한다. 예를 들어, `((Int, Int)) -> Void`는 튜플 타입 `(Int, Int)`의 단일 매개변수를 취하고 어떠한 값도 반환하지 않는 함수의 타입이다. 대조적으로, 소괄호로 둘러싸여 있지 않은 `(Int, Int) -> Void`는 두 개의 `Int` 매개변수를 취하고 어떠한 값도 반환하지 않는 함수의 타입이다. 이처럼, `Void`는 `()`의 타입 별칭이기 때문에, 함수 타입 `(Void) -> Void`는 `(()) -> ()`와 같다. 하나의 인자로 빈 튜플을 갖는 함수인 것이다. 이러한 타입은 `() -> ()`와 같지 않다. 이는 어떠한 인자도 취하지 않는다.

함수와 메소드에서 인자 이름은 대응하는 함수 타입의 일부분이 아니다.

```swift
func someFunction(left: Int, right: Int) {}
func anotherFunction(left: Int, right: Int) {}
func functionWithDifferentLabels(top: Int, bottom: Int) {}

var f = someFunction // The type of f is (Int, Int) -> Void, not (left: Int, right: Int) -> Void.
f = anotherFunction              // OK
f = functionWithDifferentLabels  // OK

func functionWithDifferentArgumentTypes(left: Int, right: String) {}
f = functionWithDifferentArgumentTypes     // Error

func functionWithDifferentNumberOfArguments(left: Int, right: Int, top: Int) {}
f = functionWithDifferentNumberOfArguments // Error
```

인자 레이블은 함수 타입의 일부분이 아니기 때문에, 함수 타입을 작성할 때 이를 생략할 수 있다.

```swift
var operation: (lhs: Int, rhs: Int) -> Int     // Error
var operation: (_ lhs: Int, _ rhs: Int) -> Int // OK
var operation: (Int, Int) -> Int               // OK
```

함수 타입이 하나의 화살표 (`→`) 이상을 포함한다면, 함수 타입은 오른쪽에서 왼쪽으로 그룹 지정된다. 예를 들어, 함수 타입 `(Int) -> (Int) -> Int`는 `(Int) -> ((Int) -> Int)`로 간주되며, 즉 `Int`를 취하고, `Int`를 취하고 `Int`를 반환하는 또다른 함수를 반환하는 함수가 되는 것이다.

에러를 던지고 다시 던질 수 있는 함수 타입은 반드시 `throws` 키워드와 함께 표시되어야 한다. `throws` 키워드는 함수 타입의 일부분이고, 에러를 던지지 않는 함수는 에러를 던지는 함수의 서브타입이다. 결과적으로, 에러를 던지지 않는 함수를 에러를 던지는 함수가 위치한 장소와 같은 곳에서 사용할 수 있다.

### 탈출하지 않는 클로저의 제한

탈출하지 않는 함수의 매개변수는 프로퍼티, 변수, 또는 `Any` 타입의 상수에 저장될 수 없다. 이는 값이 탈출하는 것을 허용하기 때문이다.

탈출하지 않는 함수의 매개변수는 또다른 탈출하지 않는 함수의 매개변수의 인자로 전달될 수 없다. 이러한 제한은 Swift가 런타임 대신 컴파일 타임에서 메모리에 대한 접근 충돌을 더 확인할 수 있게 해준다.

```swift
let external: (() -> Void) -> Void = { _ in () }
func takesTwoFunctions(first: (() -> Void) -> Void, second: (() -> Void) -> Void) {
    first { first {} }       // Error
    second { second {}  }    // Error

    first { second {} }      // Error
    second { first {} }      // Error

    first { external {} }    // OK
    external { first {} }    // OK
}
```

위의 코드에서, `takesTwoFunctions(first:second:)`의 두 개의 매개변수는 모두 함수다. 둘 다 `@escaping`으로 표시되지 않았으므로, 결과적으로 두 개 모두 탈출하지 않는다.

예제에서 "Error`라고 표시된 네 개의 함수 호출은 컴파일 에러를 발생시킨다. `first`와 `second` 매개변수가 탈출하지 않는 함수이기 때문에, 또다른 탈출하지 않는 함수의 매개변수의 인자로 전달될 수 없다. 대조적으로, "OK"라고 표시된 두 개의 함수 호출은 제한을 위반하지 않는데, `external`은 `takesTwoFunctions(first:second:)`의 매개변수 중 하나가 아니기 때문이다.

이 제한을 피할 필요가 있다면, 매개변수 중 하나를 탈출하는 것으로 표시하거나, `withoutActuallyEscaping(_:do:)`를 사용하여 탈출하지 않는 함수 매개변수 중 하나를 일시적으로 탈출하는 함수로 변환하면 된다.

## 배열 타입

Swift 언어는 Swift 표준 라이브러리의 `Array<Element>` 타입에 대하여 다음의 신택틱 슈거를 제공한다.

```swift
[{type}]
```

즉, 다음 두 개의 선언은 동일하다.

```swift
let someArray: Array<String> = ["Alex", "Brian", "Dave"]
let someArray: [String] = ["Alex", "Brian", "Dave"]
```

두 가지 경우 모두에서, `someArray` 상수는 문자열의 배열로 선언되었다. 배열의 요소들은 대괄호 안에 유효한 인덱스 값을 지정하여 서브스크립트를 통해 접근될 수 있다: `someArray[0]`은 0번째 인덱스의 요소 `"Alex"`를 참조한다.

대괄호 쌍을 중첩하여 다차원 배열을 만들 수 있으며, 이 때 요소의 기반 타입 이름은 대괄호 쌍의 가장 안 쪽에 위치한다. 예를 들어, 세 짝의 대괄호를 사용한 삼차원 정수 배열을 만들 수 있다.

```swift
var array3D: [[[Int]]] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
```

다차원 배열의 요소에 접근할 때, 가장 왼쪽에 있는 서브스크립트 인덱스는 가장 바깥쪽 배열의 인덱스에 있는 요소를 참조한다. 그 다음에 있는 서브스크립트 인덱스는 한 단계 중첩된 배열의 인덱스에 있는 요소를 참조한다. 다음 경우도 그렇다. 이는 위의 예제에서, `array3D[0]`은 `[[1, 2], [3, 4]]`, `array3D[0][1]`은 `[3, 4]`, `array3D[0][1][1]`은 `4`의 값을 참조한다는 것을 의미한다.

## 딕셔너리 타입

Swift 언어는 Swift 표준 라이브러리의 `Dictionary<Key, Value>` 타입에 대한 다음의 신택틱 슈거를 제공한다.

```swift
[{key type}: {value type}]
```

즉, 다음의 두 선언은 동일하다.

```swift
let someDictionary: [String: Int] = ["Alex": 31, "Paul": 39]
let someDictionary: Dictionary<String, Int> = ["Alex": 31, "Paul": 39]
```

두 개의 경우에서 `someDictionary` 상수는 문자열을 키로 하고 정수를 값으로 하는 딕셔너리로 선언된다.

딕셔너리의 값은 대괄호 안에 상응하는 키를 지정하여 서브스크립트를 통해 접근될 수 있다: `someDictionary["Alex"]`는 키 `"Alex"`와 연관된 값을 참조한다. 서브스크립트는 딕셔너리의 값 타입의 옵셔널 값을 반환한다. 지정된 키가 딕셔너리에 포함되어 있지 않다면, 서브스크립트는 `nil`을 반환한다.

딕셔너리의 키 타입은 반드시 Swift 표준 라이브러리의 `Hashable` 프로토콜을 준수해야 한다.

## 옵셔널 타입

Swift 언어는 접미어 `?`를 Swift 표준 라이버르리에 정의된 이름 있는 타입 `Optional<Wrapped>`의 신택틱 슈거로 정의한다. 즉 다음의 두 선언은 동일하다.

```swift
var optionalInteger: Int?
var optionalInteger: Optional<Int>
```

두 개의 경우에서 `optionalInteger` 변수는 옵셔널 정수의 타입을 갖도록 선언되었다. 타입과 `?` 사이에 공백이 없음에 주목하라.

`Optional<Wrapped>` 타입은 두 케이스, `none`과 `some(Wrapped)`를 갖는 열거형이다. 이는 값이 존재할 수도 있고 존재하지 않을 수도 있는 것을 나타내기 위해 사용된다. 어떠한 타입이라도 명시적으로, 또는 암시적으로 변환되어 옵셔널 타입으로 선언될 수 있다. 옵셔널 변수나 프로퍼티를 선언할 때 초기값을 제공하지 않는다면, 그 값은 자동으로 `nil`로 초기값이 설정된다.

옵셔널 타입의 인스턴스가 값을 포함한다면, 후위 연산자 `!`를 사용하여 값에 접근할 수 있다.

```swift
optionalInteger = 42
optionalInteger! // 42
```

`!` 연산자를 사용하여 `nil`의 값을 가지고 있는 옵셔널을 언래핑하면 런타임 에러가 발생한다.

옵셔널 표현식에 조건적으로 오퍼레이션을 수행하기 위해 옵셔널 체이팅과 옵셔널 바인딩을 사용할 수도 있다. 값이 `nil`이면 오퍼레이션이 수행되지 않고, 그러므로 런타임 에러도 발생하지 않는다.

## 암시적으로 언래핑된 옵셔널 타입

Swift 언어는 접미어 `!`를 Swift 표준 라이브러리에 정의된 이름 있는 타입 `Optional<Wrapped>`의 신택틱 슈거로 정의한다. 이는 접근될 때 자동으로 언래핑되는 동작을 추가로 제공한다. `nil`의 값을 갖는 암시적으로 언래핑된 옵셔널을 사용하려 한다면 런타임 에러가 발생하게 된다. 암시적으로 언래핑되는 동작의 예외를 갖는 다음의 두 선언은 동일하다.

```swift
var implicitlyUnwrappedString: String!
var explicitlyUnwrappedString: Optional<String>
```

타입과 `!` 사이에 공백이 없음에 주목하라.

암시적으로 언래핑하는 것은 그 타입을 포함하는 선언문의 의미를 변경하기 때문에, 튜플 타입이나 제네릭 타입 안에 중첩된 옵셔널 타입-딕셔너리나 배열과 같은 요소의 타입-은 암시적으로 언래핑된다고 표시될 수 없다.

```swift
let tupleOfImplicitlyUnwrappedElements: (Int!, Int!)  // Error
let implicitlyUnwrappedTuple: (Int, Int)!             // OK

let arrayOfImplicitlyUnwrappedElements: [Int!]        // Error
let implicitlyUnwrappedArray: [Int]!                  // OK
```

암시적으로 언래핑된 옵셔널은 옵셔널 값에 대해서 `Optional<Wrapped>` 타입과 같기 때문에, 옵셔널을 사용할 수 있는 모든 곳에서 암시적으로 언래핑된 옵셔널을 사용할 수 있다. 예를 들어, 암시적으로 언래핑된 옵셔널의 값을 옵셔널 변수, 옵셔널 상수, 옵셔널 프로퍼티에 할당할 수 있고, 반대로도 가능하다.

옵셔널처럼, 암시적으로 언래핑된 옵셔널 변수나 프로퍼티를 선언할 때 초기값을 제공하지 않는다면, 그 값은 자동으로 `nil`로 초기값이 설정된다.

암시적으로 언래핑된 옵셔널 표헌식에 대해 조건적으로 오퍼레이션을 수행하기 위해 옵셔널 체이닝을 사용하라. 값이 `nil`이면 오퍼레이션이 수행되지 않고, 그러므로 런타임 에러가 발생하지 않는다.

## 프로토콜 합성 타입

*프로토콜 합성 타입*은 지정된 프로토콜의 리스트에 있는 각각의 프로토콜을 준수하는 타입 또는 주어진 클래스의 서브클래스이고 지정된 프로토콜의 리스트에 있는 각가의 프로토콜을 준수하는 타입을 정의한다. 프로토콜 합성 타입은 오직 타입 어노테이션, 제네릭 매개변수절, 제네릭 `where`절에서만 타입을 지정하기 위해 사용될 것이다.

프로토콜 합성 타입은 다음의 형식을 갖는다.

```swift
{Protocol 1} & {Protocol 2}
```

프로토콜 합성 타입은 준수하고 싶어하는 각각의 프로토콜 타입을 상속하는 새롭고 이름 있는 프로토콜을 명시적으로 정의하지 않고, 여러 개의 프로토콜의 요구사항을 준수하는 타입의 값을 지정할 수 있게 해준다. 예를 들어, `ProtocolA`, `ProtocolB`, `ProtocolC`에서 상속된 새로운 프로토콜을 선언하는 대신, 프로토콜 합성 타입 `ProtocolA & ProtocolB & ProtocolC`를 사용할 수 있다. 이처럼, `SuperClass`의 서브클래스이고 `ProtocolA`를 준수하는 새로운 프로토콜을 선언하는 대신 `SuperClass & ProtocolA`를 사용할 수 있다.

프로토콜 합성 리스트의 각각의 아이템은 다음 중 하나를 따른다: 리스트는 최대 하나의 클래스를 포함할 수 있다.

- 클래스의 이름
- 프로토콜의 이름
- 기반 타입이 프로토콜 합성 타입, 프로토콜, 또는 클래스인 타입 별칭

프로토콜 합성 타입이 타입 별칭을 포함할 때, 같은 프로토콜이 정의에 여러 번 보이게 할 수 있다. 중복되는 것들은 무시된다. 예를 들어, 아래 코드의 `PQR`의 정의는 `P & Q & R`와 동일하다.

```swift
typealias PQ = P & Q
typealias PQR = PQ & Q & R
```

## 불투명 타입

*불투명 타입opaque type*은 기반에 있는 구체 타입을 지정하지 않고 프로토콜이나 프로토콜 합성이 준수하는 타입을 정의한다.

불투명 타입은 함수나 서브스크립트의 반환형, 또는 프로퍼티의 타입에 나타난다. 불투명 타입은 배열의 요소의 타입이나 옵셔널의 래핑된 타입과 같은 튜플 타입이나 제네릭 타입의 일부분으로 나타날 수 없다.

불투명 타입은 다음의 형식을 갖는다.

```swift
some {constraint}
```

*제약constraint*는 클래스 타입, 프로토콜 타입, 프로토콜 합성 타입, 또는 `Any`다.  불투명 타입의 인스턴스로 사용될 수 있는 값은 오직 리스트에 있는 프로토콜이나 프로토콜 합성을 준수하거나, 리스트에 있는 클래스로부터 상속받은 클래스의 타입의 인스턴스여야 한다. 불투명 값과 상호작용하는 코드는 *제약*에 의해 정의된 인터페이스의 일부분으로만 값을 사용할 수 있다.

프로토콜 선언은 불투명 타입을 포함하지 않는다. 클래스는 final이 아닌 메소드의 반환형으로 불투명 타입을 사용할 수 없다.

불투명 타입을 반환형으로 사용하는 함수는 반드시 하나의 기반 타입을 공유하는 값을 반환해야 한다. 반환형은 함수의 제네릭 타입 매개변수의 일부분이 되는 타입을 포함할 수 있다. 예를 들어, `someFunction<T>()` 함수는 `T`나 `Dictionary<String, T>` 타입의 값을 반환할 수 있다.

## 메타타입 타입

*메타타입 타입metatype type*은 클래스 타입, 구조체 타입, 열거형 타입, 프로토콜 타입을 포함한 어떠한 타입의 타입을 말한다.

클래스, 구조체 또는 열거형 타입의 메타타입은 타입 뒤에 `.Type`가 붙는 이름을 갖는다. 프로토콜 타입의 메타타입-런타임에서 프로토콜이 준수하는 구체 타입이 아닌-은 프로토콜 뒤에 `.Protocol`이 붙는 이름을 갖는다. 예를 들어, 클래스 타입 `SomeClass`의 메타타입은 `SomeClass.Type`이고, 프로토콜 `SomeProtocol`의 메타타입은 `SomeProtocol.Protocol`이다.

타입에 값으로 접근하기 위해 접미어 `self` 표현식을 사용할 수 있다. 예를 들어, `SomeClass.self`는 `SomeClass`의 인스턴스가 아닌 `SomeClass` 그 자체를 반환한다. 그리고 `SomeProtocol.self`는 런타임에서 `SomeProtocol`을 준수하는 타입의 인스턴스가 아닌 `SomeProtocol` 그 자체를 반환한다. 타입의 인스턴스에 `type(of:)` 함수를 호출하여 인스턴스의 동적이고 런타임에서의 타입에 접근할 수 있다.

```swift
class SomeBaseClass {
    class func printClassName() {
        print("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        print("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// The compile-time type of someInstance is SomeBaseClass,
// and the runtime type of someInstance is SomeSubClass
type(of: someInstance).printClassName()
// Prints "SomeSubClass"
```

타입의 메타타입 값으로부터 타입의 인스턴스를 만들기 위해 이니셜라이저 표현식을 사용할 수 있다. 클래스 인스턴스에 대하여, 호출되는 이니셜라이저는 반드시 `required` 키워드로 표시되거나 클래스 전체가 `final` 키워드로 표시되어야 한다.

```swift
class AnotherSubClass: SomeBaseClass {
    let string: String
    required init(string: String) {
        self.string = string
    }
    override class func printClassName() {
        print("AnotherSubClass")
    }
}
let metatype: AnotherSubClass.Type = AnotherSubClass.self
let anotherInstance = metatype.init(string: "some string")
```

## 셀프 타입

`Self` 타입은 특정 타입이 아니지만, 타입의 이름을 반복하거나 알지 않고도 현재 타입을 편리하게 참조할 수 있게 해준다.

프로토콜 선언이나 프로토콜 멤버 선언에서, `Self` 타입은 해당 프로토콜을 준수하는 최후의 타입을 말한다.

구조체, 클래스, 또는 열거형 선언에서, `Self` 타입은 그 선언이 도입하는 타입을 말한다. 멤버의 타입 선언에서, `Self` 타입은 해당 타입을 말한다. 클래스 선언의 멤버들에서 `Self`는 다음의 경우에만 나타날 수 있다.

- 메소드의 반환형으로
- 읽기 전용 서브스크립트의 반환형으로
- 읽기 전용 연산 프로퍼티의 타입으로
- 메소드의 구현부에서

예를 들어, 아래의 코드는 반환형이 `Self`인 인스턴스 메소드 `f`를 보여 준다.

```swift
class Superclass {
    func f() -> Self { return self }
}
let x = Superclass()
print(type(of: x.f()))
// Prints "Superclass"

class Subclass: Superclass { }
let y = Subclass()
print(type(of: y.f()))
// Prints "Subclass"

let z: Superclass = Subclass()
print(type(of: z.f()))
// Prints "Subclass"
```

예시의 마지막 부분에서 `Self`는 `z` 변수 자체의 컴파일 타임 타입 `Superclass`가 아닌 런타임 타입 `Subclass`를 말한다.

중첩 타입 선언에서, `Self` 타입은 타입 선언의 가장 안 쪽에서 도입한 타입을 말한다.

`Self` 타입은 Swift 표준 라이브러리의 `type(of:)` 함수와 같은 타입을 말한다. `Self.someStaticMember`를 작성하여 현재 타입의 멤버에 접근하는 것은 `type(of: self).someStaticMember`를 작성하는 것과 같다.

## 타입 상속 절

*타입 상속 절type inheritance clause*는 이름 있는 타입이 어떤 클래스를 상속받고 어떤 프로토콜을 준수하는지 지정하기 위해 사용된다. 타입 상속 절은 콜론 (`:`) 으로 시작하고, 그 뒤에 타입 식별자들의 리스트가 따라온다.

클래스 타입은 하나의 슈퍼클래스로부터 상속되고 여러 개의 프로토콜을 준수할 수 있다. 클래스를 정의할 때, 슈퍼클래스의 이름은 반드시 타입 식별자 리스트의 첫 번째에 나타나야 하고, 그 뒤에 클래스가 반드시 준수해야 하는 프로토콜들이 나온다. 클래스가 또다른 클래스로부터 상속받지 않는다면, 리스트는 대신 프로토콜로 시작할 수 있다.

다른 이름 있는 타입은 프로토콜의 리스트로부터만 상속받거나 준수할 수 있다. 프로토콜 타입은 다른 여러 개의 프로토콜을 상속할 수 있다. 프로토콜 타입이 다른 프로토콜로부터 상속받았다면, 그러한 다른 프로토콜들의 요구사항 집합은 함께 결합되고, 현재 프로토콜을 상속한 어떠한 타입은 반드시 그러한 요구사항을 모두 준수해야 한다.

열거형 정의에서의 타입 상속 절은 프로토콜의 리스트나, 원시 값을 케이스에 할당하는 열거형의 경우에는 원시 값의 타입을 지정하는 하나의 이름 있는 타입일 수 있다.

## 타입 추론

Swift는 광범위하게 *타입 추론*을 사용하여 코드에서 타입이나 많은 변수와 표현식의 타입을 생략할 수 있게 해준다. 예를 들어, `var x: Int = 0`이라고 작성하는 대신 `var x = 0`이라고 작성할 수 있으며, 완전하게 타입을 생략할 수 있다-컴파일러는 `x`라는 이름의 값의 타입을 `Int`라고 추론한다. 비슷하게, 완전한 타입이 컨텍스트로부터 추론될 수 있을 때 타입의 일부분을 생략할 수 있다. 예를 들어, `let dict: Dictionary = ["A": 1]`이라고 작성한다면, 컴파일러는 `dict`가 `Dictionary<String, Int>` 타입을 갖는다고 추론한다.

위의 두 가지 예시에서, 타입 정보는 표현식 트리의 이파리에서 루트까지 넘겨진다. 즉, `var x: Int = 0`에서 `x`의 타입은 먼저 `0`의 타입을 확인한 후 이 타입 정보를 루트 (변수 `x`) 까지 넘겨주면서 추론된다.

Swift에서, 타입 정보는 반대 방향-루트에서 이파리-으로 흐를 수도 있다. 예를 들어, 다음 예시에서 상수 `eFloat`에 대한 명시적인 타입 어노테이션 (`: Float`) 는 숫자 리터럴 `2.71828`이 `Double` 대신 `Float` 타입으로 추론되도록 한다.

```swift
let e = 2.71828 // The type of e is inferred to be Double.
let eFloat: Float = 2.71828 // The type of eFloat is Float.
```

Swift에서 타입 추론은 하나의 표현식 또는 구문의 수준에서 동작한다. 이는 표현식에서 생략된 타입이나 타입의 일부분을 추론하기 위해 필요한 모든 정보는 그 표현식이나 그 하위 표현식들 중 하나의 타입을 확인하는 것에서 반드시 접근 가능해야 함을 의미한다.