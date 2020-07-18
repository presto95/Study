# 제네릭 매개변수 및 인자

이 챕터는 제네릭 타입, 함수, 이니셜라이저에 대한 매개변수 및 인자에 대해 기술한다. 제네릭 타입, 함수, 서브스크립트 또는 이니셜라이저를 선언할 때, 제네릭 타입, 함수, 또는 이니셜라이저가 함께 작동할 수 있는 타입 매개변수를 지정한다. 이러한 타입 매개변수는 제네릭 타입의 인스턴스가 만들어지거나 제네릭 함수 또는 이니셜라이저가 호출될 때 실제 구체 타입 인자에 의해 교체되는 플레이스홀더의 역할을 한다.

## 제네릭 매개변수 절

*제네릭 매개변수 절*은 제네릭 타입이나 함수의 타입 매개변수를 지정하며, 뒤에 그러한 매개변수에 대한 관련 제약과 요구조건이 뒤따른다. 제네릭 매개변수 절은 화살 괄호 (`<>`) 로 둘러싸여 있으며 다음의 형식을 갖는다.

```swift
<{generic parameter list}>
```

*generic parameter list*는 제네릭 매개변수들의 콤마로 구분된 리스트이며, 각각은 다음의 형식을 갖는다.

```swift
{type parameter}: {constraint}
```

제네릭 매개변수는 *type parameter* 뒤에 선택적인 *constraint*가 뒤따라오는 것으로 구성된다. *type parameter*는 단순히 플레이스홀더 타입의 이름이다 (예를 들어, `T`, `U`, `V`, `Key`, `Value` 등). 함수나 이니셜라이저의 시그니쳐에서를 포함하여, 타입, 함수, 또는 이니셜라이저 선언의 나머지 부분에서 타입 매개변수 (또는 그 관련 타입들) 에 접근한다.

*constraint*는 타입 매개변수가 특정 클래스에서 상속받거나 프로토콜 또는 프로토콜 합성을 준수한다는 것을 지정한다. 예를 들어, 아래의 제네릭 함수에서, 제네릭 매개변수 `T: Comparable`은 해당 타입 매개변수 `T`를 교체하는 타입 인자는 반드시 `Comparable` 프로토콜을 채택해야 한다는 것을 가리킨다.

```swift
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

예를 들어, `Int`와 `Double`은 `Comparable` 프로토콜을 준수하기 때문에, 이 함수는 두 가지 타입 중 하나의 인자를 받는다. 제네릭 타입과는 대조적으로, 제네릭 함수나 이니셜라이저를 사용할 때 제네릭 인자 절을 지정하지 않는다. 타입 인자는 함수나 이니셜라이저에 넘겨진 인자의 타입으로부터 대신 추론된다.

```swift
simpleMax(17, 42) // T is inferred to be Int
simpleMax(3.14159, 2.71828) // T is inferred to be Double
```

### 제네릭 where 절

타입이나 함수의 구현부의 열린 중괄호 바로 앞에 제네릭 `where` 절을 포함하여 타입 매개변수와 그것의 관련 타입에 추가 요구조건을 지정할 수 있다. 제네릭 `where` 절은 `where` 키워드 뒤에 하나 이상의 콤마로 구분된 *requirements*로 구성된다.

```swift
where {requirements}
```

제네릭 `where` 절에서 *requirements*는 해당 타입 매개변수가 클래스로부터 상속받았거나 프로토콜 또는 프로토콜 합성을 준수한다는 것을 지정한다. 제네릭 `where` 절이 타입 매개변수에 대한 간단한 제약을 표현하기 위한 신택틱 슈거를 제공한다 할지라도 (예를 들어, `<T: Comparable>`은 `<T> where T: Comparable` 등과 동일함), 타입 매개변수와 그것의 연관 타입에 더 복잡한 제약을 제공하기 위해 이를 사용할 수 있다. 예를 들어, 프로토콜을 준수하기 위해 타입 매개변수의 연관 타입에 제약을 줄 수 있다. 예를 들어, `<S: Sequence> where S.Iterator.Element: Equatable`은 `S`가 `Sequence` 프로토콜을 준수하고 연관 타입 `S.Iterator.Element`는 `Equatable` 프로토콜을 준수한다는 것을 지정한다. 이 제약은 시퀀스의 각 요소가 동등 비교 가능하다는 것을 보장한다.

또한 `==` 연산자를 사용하여 두 개의 타입이 동일하다는 요구조건을 지정할 수 있다. 예를 들어, `<S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element`는 `S1`과 `S2`가 `Sequence` 프로토콜을 채택하고 두 시퀀스의 요소는 반드시 같은 타입이어야 한다는 제약을 표현한다.

타입 매개변수에 대해 교체된 타입 인자는 반드시 타입 매개변수에 놓여진 모든 제약과 요구조건을 만족해야 한다.

제네릭 `where` 절은 타입 매개변수를 포함하는 선언의 일부분으로, 또는 타입 매개변수를 포함하는 선언의 내부에 중첩된 선언의 일부분으로 나타날 수 있다. 중첩 선언에 대한 제네릭 `where` 절은 여전히 감싸진 선언의 타입 매개변수를 참조할 수 있다; 그러나, 해당 `where` 절로부터의 요구조건은 오직 그것이 작성된 선언에서만 적용된다.

감싸진 선언 또한 `where` 절을 가지고 있다면, 두 개의 절에 있는 요구조건들은 혼합된다. 아래의 예시에서, `startWithZero()`는 오직 `Element`가 `SomeProtocol`과 `Numeric`을 모두 준수할 때만 사용 가능하다.

```swift
extension Collection where Element: SomeProtocol {
    func startsWithZero() -> Bool where Element: Numeric {
        return first == .zero
    }
}
```

타입 매개변수에 대해 다른 제약이나 요구조건, 또는 둘 다를 제공하여 제네릭 함수나 이니셜라이저를 오버로딩할 수 있다. 오버로딩된 제네릭 함수나 이니셜라이저를 호출할 때, 컴파일러는 이러한 제약들을 사용하여 어떤 오버로딩된 함수나 이니셜라이저를 호출해야 할지를 결정한다.

## 제네릭 인자 절

*제네릭 인자 절*은 제네릭 타입의 타입 인자를 지정한다. 제네릭 인자 절은 화살 괄호 (`<>`) 로 둘러싸여 있고 다음의 형식을 갖는다.

```swift
<{generic argument list}>
```

*generic argument list*는 타입 인자의 콤마로 구분된 리스트다. *타입 인자*는 제네릭 타입의 제네릭 매개변수 절에 있는 대응하는 타입 매개변수를 교체하는 실제 구체 타입의 이름이다. 그 결과는 제네릭 타입의 특화된 버전이다. 아래의 예시는 Swift 표준 라이브러리의 제네릭 딕셔너리 타입의 단순화된 버전을 보여준다.

```swift
struct Dictionary<Key: Hashable, Value>: Collection, ExpressibleByDictionaryLiteral {
    /* ... */
}
```

제네릭 `Dictionary` 타입의 특화된 버전 `Dictionary<String, Int>`는 제네릭 매개변수 `Key: Hashable`과 `Value`를 구체 타입 인자 `String`과 `Int`로 교체하여 형성된다. 각각의 타입 인자는 제네릭 `where` 절에 지정된 추가 요구조건을 포함하여, 반드시 그것이 교체하는 제네릭 매개변수의 모든 제약을 충족해야 한다. 위의 예시에서, `Key` 타입 매개변수는 `Hashable` 프로토콜을 준수하도록 제약이 걸려 있으며, 그러므로 `String`도 반드시 `Hashable` 프로토콜을 준수해야 한다.

타입 매개변수를 그 자체의 제네릭 타입의 특화된 버전 (적절한 제약과 요구조건을 충족시켜 제공됨) 인 타입 인자로 교체할 수도 있다. 예를 들어, `Array<Element>`에 있는 타입 매개변수 `Element`를 배열의 특화된 버전 `Array<Int>`로 교체할 수 있고, 이는 요소가 정수 배열을 갖는 배열을 형성한다.

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```