# 구문

Swift에는 세 가지 종류의 구문이 있다: 간단한 구문, 컴파일러가 제어하는 구문, 그리고 제어 흐름 구문이다. 간단한 구문은 가장 일반적이며 표현식 또는 선언문으로 구성된다. 컴파일러가 제어하는 구문은 프로그램이 컴파일러 동작의 양상에서 변화하게 해주며, 조건적 컴파일 블록과 라인 제어 구문을 포함한다.

제어 흐름 구문은 프로그램에서 실행 흐름을 제어하기 위해 사용된다. Swift에는 여러 가지 제어 흐름 구문 타입이 있으며, 루프 구문, 분기 구문, 제어 전달 구문을 포함한다. 루프 구문은 코드 블록이 반복적으로 실행될 수 있게 해주며, 분기 구문은 코드의 특정 구문이 오직 특정 조건이 충족되었을 때만 실행될 수 있게 해주며, 제어 전달 구문은 코드가 실행되는 순서를 변경할 수 있는 방법을 제공한다. 게다가, Swift는 스코프를 도입하고, 에러를 잡고 처리하기 위한 `do` 구문을 제공하며, 현재 스코프에서 빠져나가기 직전에 클린업 액션을 실행하기 위한 `defer` 구문도 제공한다.

세미콜론 (`;`) 은 구문 뒤에 선택적으로 붙을 수 있으며, 여러 개의 구문이 같은 라인에 나타날 때 이들을 구분하기 위해 사용된다.

## 루프 구문

루프 구문은 코드 블록이 루프에 지정된 조건에 따라 반복적으로 실행될 수 있게 해준다. Swift는 세 개의 루프 구문: `for-in` 구문, `while` 구문, `repeat-while` 구문을 갖고 있다.

루프 구문에 있는 제어 흐름은 `break` 구문과 `continue` 구문에 의해 변경될 수 있다.

### For-In 구문

`for-in` 구문은 코드 블록이 `Sequence` 프로토콜을 채택하는 컬렉션 (또는 어떠한 타입이든) 의 각각의 아이템에 대해 한 번 실행되게 해준다.

`for-in` 구문은 다음의 형식을 갖는다.

```swift
for {item} in {collection} {
    {statements}
}
```

`makeIterator()` 메소드는 *collection* 표현식이 이터레이터 타입, 즉 `IteratorProtocol` 프로토콜을 준수하는 타입의 값을 획득하기 위해 호출된다. 프로그램은 이터레이터의 `next()` 메소드를 호출하여 루프 실행을 시작한다. 이 값이 `nil`을 반환하지 않으면 *item* 패턴에 할당되고, 프로그램은 *statements*를 실행하며, 루프의 시작 부분에서 실행을 지속한다. 그렇지 않으면, 프로그램은 할당을 수행하거나 *statements*를 실행하는 것을 하지 않고, `for-in` 구문의 실행을 종료한다.

### While 구문

`while` 구문은 조건이 참으로 남아 있는 한 코드 블록이 반복적으로 실행될 수 있게 해준다.

`while` 구문은 다음의 형식을 갖는다.

```swift
while {condition} {
    {statements}
}
```

`while` 구문은 다음과 같이 실행된다.

1. *condition*이 평가된다. `true`이면 2단계로 넘어가 실행을 지속한다. `false`이면 프로그램은 `while` 구문의 실행을 종료한다.
2. 프로그램은 *statements*를 실행하고, 실행은 1단계로 되돌아간다.

*condition*의 값은 *statements*가 실행되기 전에 평가되므로, `while` 구문에 있는 *statements*는 0번 이상 실행될 수 있다.

*condition*의 값은 반드시 `Bool` 타입이거나 `Bool`로 브릿징되는 타입이어야 한다. 조건은 또한 옵셔널 바인딩 선언일 수 있다.

### Repeat-Whilte 구문

`repeat-while` 구문은 조건이 참으로 남아 있는 한 코드 블록이 한 번 이상 실행될 수 있게 해준다.

`repeat-while` 구문은 다음의 형식을 갖는다.

```swift
repeat {
    {statements}
} while {condition}
```

`repeat-while` 구문은 다음과 같이 실행된다.

1. 프로그램은 *statements*를 실행하고, 실행은 2단계를 지속한다.
2. *condition*이 평가된다. `true`이면 1단계로 되돌아간다. `false`이면 프로그램은 `repeat-while` 구문의 실행을 종료한다.

*condition*의 값은 *statements*가 실행된 후에 평가되므로, `repeat-while` 구문에 있는 *statements*는 적어도 한 번은 실행된다.

*condition*의 값은 반드시 `Bool` 타입이거나 `Bool`로 브릿징되는 타입이어야 한다. 조건은 또한 옵셔널 바인딩 선언일 수 있다.

## 분기 구문

분기 구문은 프로그램이 하나 이상의 조건들의 값에 따라 코드의 특정 부분을 실행할 수 있게 해준다. 분기 구문에 지정된 조건들의 값들은 프로그램이 어떻게 분기되는지를 제어하며, 그러므로 어떤 코드 블록이 실행될지를 제어한다. Swift는 세 개의 분기 구문, `if` 구문, `guard` 구문, `switch` 구문을 갖고 있다.

`if` 구문 또는 `switch` 구문에서의 제어 흐름은 `break` 구문에 의해 변경될 수 있다.

### If 구문

`if` 구문은 하나 이상의 조건의 평가에 기반한 코드 실행을 위해 사용된다.

`if` 구문에는 두 가지 기본 형식이 있다. 각각의 형식에서, 여는 중괄호와 닫는 중괄호가 요구된다.

첫 번째 형식은 코드가 오직 조건이 참일 때만 실행되게 해주며, 다음의 형식을 갖는다.

```swift
if {condition} {
    {statements}
}
```

`if` 구문의 두 번째 형식은 (`else` 키워드가 도입하는) 추가적인 `else` 절을 제공하며, 조건이 참일 때 실행할 코드와 같은 조건이 거짓일 때 실행할 또다른 코드를 위해 사용된다. 하나의 else 절이 나타날 때, `if` 구문은 다음의 형식을 갖는다.

```swift
if {condition} {
    {statements to execute if condition is true}
} else {
    {statements to execute if condition is false}
}
```

`if` 문의 else 절은 하나 이상의 조건을 테스트하기 위해 또다른 `if` 구문을 포함할 수 있다. 이러한 방식으로 함께 체이닝된 `if` 구문은 다음의 형식을 갖는다.

```swift
if {condition 1} {
    {statements to execute if condition 1 is true}
} else if {condition 2} {
    {statements to execute if condition 2 is true}
} else {
    {statements to execute if both conditions are false}
}
```

`if` 구문의 조건 값은 반드시 `Bool` 타입이거나 `Bool`로 브릿징되는 타입이어야 한다. 조건은 또한 옵셔널 바인딩 선언일 수 있다.

### Guard 구문

`guard` 구문은 하나 이상의 조건이 충족되지 않았을 때 스코프 밖으로 프로그램의 제어를 전달하기 위해 사용된다.

`guard` 구문은 다음의 형식을 갖는다.

```swift
guard {condition} else {
    {statements}
}
```

`guard` 구문에 있는 조건의 값은 반드시 `Bool` 타입이거나 `Bool`로 브릿징되는 타입이어야 한다. 조건은 또한 옵셔널 바인딩 선언일 수 있다.

`guard` 구문에서 옵셔널 바인딩 선언에서의 값이 할당된 어떠한 상수나 변수는 나머지 guard 구문의 감싸진 스코프에서 사용될 수 있다.

`guard` 구문의 `else` 절은 필수이며, 반드시 `Never` 반환형을 갖는 함수를 호출하거나, 다음의 구문 중 하나를 사용하여 guard 구문의 스코프 밖으로 프로그램 제어를 전달해야 한다.

- `return`
- `break`
- `continue`
- `throw`

### Switch 구문

`switch` 구문은 특정 코드 블록이 제어 표현식의 값에 따라 실행되게 해준다.

`switch` 구문은 다음의 형식을 갖는다.

```swift
switch {control expression} {
case {pattern 1}:
    {statements}
case {pattern 2} where {condition}:
    {statements}
case {pattern 3} where {condition},
     {pattern 4} where {condition}:
    {statements}
default:
    statements
}
```

`switch` 구문의 *control expression*은 평가되고 나서 각 케이스에 지정된 패턴과 비교된다. 매칭되는 것이 있다면, 프로그램은 해당 케이스 스코프에 리스팅된 *statements*를 실행한다. 각 케이스의 스코프는 비어 있을 수 없다. 결과적으로, 각 케이스 레이블의 콜론 (`:`) 뒤에 적어도 하나의 구문을 반드시 포함해야 한다. 매칭되는 케이스의 구현부에서 어떠한 코드도 실행하고 싶지 않다는 것을 의도한다면 하나의 `break` 구문을 사용하면 된다.

코드가 분기할 수 있는 표현식들의 값들은 매우 유연하다. 예를 들어, 정수와 문자와 같은 스칼라 타입의 값들 뿐만 아니라, 부동소수점 숫자, 문자열, 튜플, 커스텀 클래스의 인스턴스, 그리고 옵셔널을 포함한 어떠한 타입의 값에 대해서도 코드를 분기할 수 있다. *control expression*의 값은 심지어 열거형의 케이스의 값에 매칭될 수 있고, 값의 특정 범위에 포함되어 있는지를 확인할 수도 있다.

`switch` 케이스는 각 패턴 뒤에 선택적으로 `where` 절을 포함할 수 있다. *where 절*은 표현식 다음에 오는 `where` 키워드로 도입되며, 케이스에 있는 패턴이 *control expression*에 매칭되는지를 고려하기 전에 추가적인 조건을 제공하기 위해 사용된다. `where` 절이 있다면, 관련 케이스에 있는 *statements*는 *control expression*의 값이 해당 케이스의 패턴들 중 하나를 충족하고 `where` 절의 표현식이 `true`로 평가될 때만 실행된다. 예를 들어, 아래의 예시에서 *control expression*은 `(1, 1)`처럼 같은 값을 가진 두 개의 요소를 포함하는 튜플일 때만 해당 케이스에 매칭된다.

```swift
case let (x, y) where x == y:
```

위의 예시에서 볼 수 있듯, 케이스에 있는 패턴은 `let` 키워드를 사용하여 상수에 (`var` 키워드를 사용하여 변수에 바인딩할 수도 있음) 바인딩할 수도 있다. 이러한 상수들 (또는 변수들) 은 이후에 상응하는 `where` 절에서 참조될 수 있고, 케이스의 스코프 내에 있는 나머지 코드에서도 참조될 수 있다. 해당 케이스가 제어 표현식에 매칭되는 여러 개의 패턴을 포함한다면, 모든 패턴은 반드시 같은 상수 또는 변수 바인딩을 포함해야 하며, 각각의 바인딩된 변수나 상수는 반드시 모든 케이스의 패턴에서 같은 타입을 가져야 한다.

`switch` 구문은 또한 디폴트 케이스를 포함할 수 있으며, `default` 키워드로 도입된다. 디폴트 케이스 안에 있는 코드는 제어 표현식에 매칭되는 다른 케이스가 없을 때만 실행된다. `switch` 구문은 오직 하나의 디폴트 케이스를 포함할 수 있으며, 이는 반드시 `switch` 구문의 마지막에 나타나야 한다.

패턴-매칭 연산자의 실제 실행 순서와, 특히 케이스에 있는 패턴들의 평가 순서가 지정되지 않았을지라도, `switch` 구문에 있는 패턴 매칭은 평가가 소스 순서로 수행되는 것처럼, 즉, 소스 코드에서 나타나는 순서대로 작동한다. 결과적으로, 여러 개의 케이스가 같은 값으로 평가되는 패턴을 포함한다면, 따라서 제어 표현식의 값에 매칭될 수 있다면, 프로그램은 소스 순서에서 첫 번째의 매칭되는 케이스에 있는 코드만을 실행한다.

**Switch 구문은 반드시 완전해야*exhaustive* 한다**

Swift에서, 제어 표현식의 타입의 모든 가능한 값은 적어도 케이스의 한 패턴의 값에 매칭되어야 한다. 이것이 단순하게 충족되지 않았다면 (예를 들어, 제어 표현식의 타입이 `Int`일 때), 이 요구사항을 만족하기 위해 디폴트 케이스를 포함할 수 있다.

**향후 열거형 케이스를 스위칭하기**

*동결되지 않은 열거형nonfrozen enumeration*은 향후, 심지어 앱을 컴파일하고 배포한 후에도 새로운 열거형 케이스를 얻을 가능성이 있는 특별한 종류의 열거형이다. 동결되지 않은 열거형을 스위칭하는 것은 별도의 고려가 필요하다. 라이브러리의 저자가 열거형을 동결되지 않았다고 표시한다면, 그들은 새로운 열거형 케이스를 추가할 수 있는 권리를 남겨둔 것이며, 그 열거형과 상호작용하는 어떠한 코드라도 *반드시* 다시 컴파일되지 않고도 그러한 향후 케이스를 처리할 수 있어야 한다. 라이브러리 발전 모드에서 컴파일된 코드, 표준 라이브러리에 있는 코드, Apple 프레임워크를 위한 Swift 오버레이, 그리고 C 및 Objective-C 코드는 동결되지 않은 열거형을 선언할 수 있다.

동결되지 않은 열거형 값을 스위칭할 때, 모든 열거형의 케이스가 이미 상응하는 switch 케이스를 갖고 있다 할지라도, 항상 디폴트 케이스를 포함해야 할 필요가 있다. 디폴트 케이스에 `@unknown` 애트리뷰트를 붙여서 해당 디폴트 케이스는 향후 추가될 열거형 케이스에 대해서만 매칭된다는 것을 가리킬 수 있다. Swift는 디폴트 케이스가 컴파일 타임에 알려진 어떠한 열거형 케이스와 매칭된다면 경고를 낸다. 이 향후의 경고는 당신에게 라이브러리 저자가 추가한 열거형의 새로운 케이스가 이에 상응하는 switch 케이스를 가지고 있지 않는다는 것을 알려 준다.

다음의 예시는 표준 라이브러리의 `Mirror.AncestorRepresentation` 열거형에 존재하는 세 가지 케이스를 스위칭한다. 향후 케이스를 추가한다면, 컴파일러는 당신에게 새로운 케이스를 고려하기 위해 switch 구문을 업데이트할 필요가 있다는 경고를 발생시킨다.

```swift
let representation: Mirror.AncestorRepresentation = .generated
switch representation {
case .customized:
    print("Use the nearest ancestor’s implementation.")
case .generated:
    print("Generate a default mirror for all ancestor classes.")
case .suppressed:
    print("Suppress the representation of all ancestor classes.")
@unknown default:
    print("Use a representation that was unknown when this code was compiled.")
}
// Prints "Generate a default mirror for all ancestor classes."
```

**실행은 암시적으로 케이스를 통과하여 떨어지지*fallthrough* 않는다.**

매칭된 케이스 안의 코드 실행이 완료된 후, 프로그램은 `switch` 구문을 빠져나간다. 프로그램 실행은 다음 케이스나 디폴트 케이스로 지속하거나 "통과하여 떨어지지*fallthrough*" 않는다. 즉, 어떤 케이스에서 다음 케이스로 실행을 지속하고 싶다면, 단순히 `fallthrough` 키워드로만 구성된 `fallthrough` 구문을 명시적으로 포함하여 실행을 지속하게 하면 된다.

## 레이블링된 구문

루프 구문, `if` 구문, `switch` 구문, 또는 *구문 레이블*이 있는 `do` 구문에 접두어를 붙일 수 있으며, 레이블 이름 바로 뒤에 콜론 (`:`) 이 붙는 것으로 구성된다. `break`와 `continue` 구문에 구문 레이블을 사용하여 루프 구문이나 `switch` 구문에서 제어 흐름을 어떻게 변경하고 싶은지에 대해 명시할 수 있다.

레이블링된 구문의 스코프는 구문 레이블 뒤에 나오는 전체 구문이다. 레이블링된 구문을 중첩할 수 있으나, 각 구문 레이블의 이름은 반드시 유일해야 한다.

## 제어 전달 구문

제어 전달 구문은 프로그램 제어를 코드의 한 부분에서 또 다른 부분으로 무조건적으로 전달하여 프로그램의 코드가 실행되는 순서를 변경할 수 있다. Swift는 다섯 개의 구문: `break` 구문, `continue` 구문, `fallthrough` 구문, `return` 구문, `throw` 구문을 갖는다.

### Break 구문

`break` 구문은 루프, `if` 구문, 또는 `switch` 구문의 프로그램 실행을 종료한다. `break` 구문은 오직 `break` 키워드로만 구성되거나, 아래에 보이는 것처럼 `break` 키워드 다음에 구문 레이블의 이름을 붙인 것으로 구성될 수 있다.

```swift
break
break {label name}
```

`break` 구문 다음에 구문 레이블의 이름이 올 때, 루프, `if` 구문 또는 해당 레이블로 이름 지어진 `switch` 구문의 프로그램을 종료한다.

`break` 구문 뒤에 구문 레이블의 이름이 오지 않을 때, `switch`구문 또는 이것이 발생하는 가장 안쪽의 루프 구문의 프로그램 실행을 종료한다. `if` 구문을 빠져나가기 위해 레이블링되지 않은 `break` 구문을 사용할 수 없다.

두 개의 케이스 모두에서, 프로그램 제어는 감싸진 루프나 `switch` 구문 다음에 오는 코드가 있다면, 그것의 첫 번째 라인으로 전달된다.

### Continue 구문

`continue` 구문은 루프 구문의 현재 반복의 프로그램 실행을 종료하지만 루프 구문의 실행을 멈추지는 않는다. `continue` 구문은 오직 `continue` 키워드로만 구성되거나, 아래에 보이는 것처럼 `continue` 키워드 다음에 구문 레이블의 이름을 붙인 것으로 구성될 수 있다.

```swift
continue
continue {label name}
```

`continue` 구문 다음에 구문 레이블의 이름이 올 때, 해당 레이블로 이름 지어진 루프 구문의 현재 반복의 프로그램 실행을 종료한다.

`continue` 구문 뒤에 구문 레이블의 이름이 오지 않을 때, 이것이 발생하는 가장 안쪽의 루프 구문의 현재 반복의 프로그램 실행을 종료한다.

두 개의 케이스 모두에서, 프로그램 제어는 감싸진 루프 구문의 조건으로 전달된다.

`for` 구문에서, 증감 표현식은 여전히 `continue` 구문이 실행된 후에 평가되는데, 증감 표현식은 루프의 구현부의 실행 이후에 평가되기 때문이다.

### Fallthrough 구문

`fallthrough` 구문은 `fallthrough` 키워드로 구성되며 오직 `switch` 구문의 케이스 블록 안에서만 발생한다. `fallthrough` 구문은 `switch` 구문의 한 케이스에서 다음 케이스로 프로그램 실행을 지속하게 한다. 프로그램 실행은 다음 케이스 레이블의 패턴이 `switch` 구문의 제어 표현식의 값과 매칭되지 않더라도 다음 케이스로 지속된다.

`fallthrough` 구문은 케이스 `switch` 구문 안이라면 케이스 블록의 마지막 구문이라도 어디든 나타날 수 있지만, 마지막 케이스 블록 안에서는 사용될 수 없다. 또한 케이스 패턴이 값 바인딩 패턴을 포함하고 있다면 해당 케이스 블록으로 제어를 전달할 수 없다.

### Return 구문

`return` 구문은 함수나 메소드 정의의 구현부에서 발생하며, 프로그램 실행이 호출한 함수나 메소드에서 반환하도록 한다. 프로그램 실행은 함수 또는 메소드 호출 직후의 포인트에서 지속된다.

`return` 구문은 오직 `return` 키워드로만 구성되거나, 아래에 보이는 것처럼 `return` 키워드 다음에 오는 표현식으로 구성될 수 있다.

```swift
return
return {expression}
```

`return` 구문 다음에 표현식이 올 때, 표현식의 값은 호출한 함수나 메소드에서 반환된다. 표현식의 값이 함수나 메소드의 선언에서 선언된 반환형의 값과 일치하지 않는다면, 표현식의 값은 호출한 함수나 메소드에서 반환되기 전에 반환형으로 변환된다.

- 알아두기

  실패 가능한 이니셜라이저에서 기술한 것처럼, `return` 구문의 특별한 형태 (`return nil`) 은 초기화 실패를 가리키기 위해 실패 가능한 이니셜라이저에서 사용될 수 있다.

`return` 구문 다음에 표현식이 오지 않을 때, 오직 함수나 메소드가 값을 반환하지 않고 반환하게 하기 위해 사용될 수 있다. (즉, 함수나 메소드의 반환형이 `Void` 또는 `()`인 것)

### Throw 구문

`throw` 구문은 에러를 던지는 함수나 메소드의 구현부에서 발생하거나, 타입이 `throws` 키워드로 표시된 클로저 표현식의 구현부에서 발생한다.

`throw` 구문은 프로그램이 현재 스코프에서의 실행을 종료하고 그것이 감싼 스코프에서 에러를 전파하는 것을 시작하는 원인이 된다. 던져진 에러는 `do` 구문의 `catch` 절에서 처리될 때까지 전파를 지속한다.

`throw` 구문은 `throw` 키워드 다음에 표현식이 오는 것으로 구성되며, 아래와 같다.

```swift
throw {expression}
```

*expression*의 값은 반드시 `Error` 프로토콜을 준수하는 타입이어야 한다.

### Defer 구문

`defer` 구문은 `defer` 구문이 나타나는 스코프의 바깥으로 프로그램 제어를 전달하기 바로 직전에 코드를 실행하기 위해 사용된다.

`defer` 구문은 다음의 형식을 갖는다.

```swift
defer {
    {statements}
}
```

`defer` 구문 안에 있는 구문은 프로그램 제어가 어떻게 전달되는지에 상관 없이 실행된다. 이는 예를 들어 `defer` 구문은 파일 디스크립터를 닫는 것과 같은 수동 리소스 관리를 수행하기 위해, 그리고 에러가 던져질지라도 발생할 필요가 있는 액션을 수행해야 할 때 사용될 수 있다는 것을 의미한다.

여러 개의 `defer` 구문이 같은 스코프에 나타난다면, 그것들이 나타나는 순서는 실행되는 순서의 역순이다. 주어진 스코프에서 가장 마지막에 있는 `defer` 구문이 첫 번째로 실행된다는 것은 마지막 `defer` 구문에 있는 구문이 다른 `defer` 구문에 의해 클린업될 리소스를 참조할 수 있다는 것을 의미한다.

```swift
func f() {
    defer { print("First defer") }
    defer { print("Second defer") }
    print("End of function")
}
f()
// Prints "End of function"
// Prints "Second defer"
// Prints "First defer"
```

`defer` 구문에 있는 구문은 `defer` 구문의 바깥으로 프로그램 제어를 전달할 수 없다.

### Do 구문

`do` 구문은 새로운 스코프를 도입하기 위해 사용되고, 선택적으로 하나 이상의 `catch` 절을 포함하여 정의된 에러 조건과 일치하는 패턴을 포함할 수 있다. `do` 구문의 스코프 안에 선언된 변수와 상수는 오직 해당 스코프 안에서만 접근될 수 있다.

Swift에서 `do` 구문은 C에서 코드 블록의 한계를 정하기 위해 사용된 중괄호 (`{}`) 와 비슷하며, 런타임에서 성능 비용을 초래하지 않는다.

`do` 구문은 다음의 형식을 따른다.

```swift
do {
    try {expression}
    {statements}
} catch {pattern 1} {
    {statements}
} catch {pattern 2} where {condition} {
    {statements}
} catch {pattern 3}, {pattern 4} where {condition} {
    {statements}
} catch {
    {statements}
}
```

`do` 코드 블록에 있는 어떠한 구문이 에러를 던진다면, 프로그램 제어는 패턴이 에러와 매칭되는 첫 번째 `catch`절로 전달된다. 어떠한 절도 매칭되지 않는다면, 에러는 주변 스코프로 전파된다. 에러가 최상위 수준에서 처리되지 않는다면, 프로그램 실행은 런타임 에러와 함께 중단된다.

`switch` 구문처럼, 컴파일러는 `catch`절이 완전해야 한다는 것을 추론하는 것을 시도한다. 그러한 결정이 만들어질 수 있다면, 에러는 처리된 것으로 간주된다. 그렇지 않으면 에러는 포함하는 스코프 밖으로 전파될 수 있으며, 이는 에러가 반드시 감싸진 `catch`절에 의해 처리되거나, 포함하는 함수가 `throws`와 함께 선언되어야 한다는 것을 의미한다.

여러 개의 패턴을 갖는 `catch`절은 그 패턴들 중 어떠한 것이라도 해당 에러와 매칭된다면 에러와 매칭된다. `catch`절이 여러 개의 패턴을 포함한다면, 모든 패턴은 반드시 같은 상수 또는 변수 바인딩을 포함해야 하며, 각각의 바인딩된 변수나 상수는 반드시 `catch`절의 패턴들에 있는 것과 같은 타입을 가져야 한다.

에러가 처리되었다는 것을 보장하기 위해 와일드카드 패턴 (`_`) 과 같이 모든 에러와 매칭되는 패턴과 함께 `catch`절을 사용할 수 있다. `catch`절이 패턴을 명시하지 않는다면, 해당 `catch`절은 어떠한 에러와도 매칭되고,  `error`라는 이름을 가진 지역 상수와 바인딩된다.

## 컴파일러 제어 구문

컴파일러 제어 구문은 프로그램이 컴파일러 동작의 양상을 변경할 수 있게 해준다. Swift는 세 개의 컴파일러 제어 구문: 조건적 컴파일 블록, 라인 제어 구문, 그리고 컴파일 타임 진단 구문을 가지고 있다.

### 조건적 컴파일 블록

조건적 컴파일 블록은 코드가 하나 이상의 컴파일 조건의 값에 따라 조건적으로 컴파일될 수 있게 해준다.

모든 조건적 컴파일 블록은 `#if` 컴파일 지시자로 시작하여 `endif` 컴파일 지시자로 끝난다. 단순한 조건적 컴파일 블록은 다음의 형식을 따른다.

```swift
#if {compilation condition}
{statements}
#endif
```

`if` 구문의 조건과는 다르게, *compilation condition*은 컴파일 타임에 평가된다. 결과적으로, *statements*는 오직 *compilation condition*이 컴파일 타임에 `true`로 평가될 때만 컴파일되고 실행된다.

*compilation condition*은 `true`와 `false` 불리언 리터럴, `-D` 커맨드 라인 플래그와 함께 사용되는 식별자, 또는 아래의 표에 작성된 플랫폼 조건들 중 어떠한 것을 포함할 수 있다.

[플랫폼 조건](https://www.notion.so/9c96c202a95b41c8bd84d920c9cdb216)

`swift()`와 `compiler()` 플랫폼 조건을 위한 버전 번는 메이저 번호, 선택적인 마이너 번호, 선택적인 패치 번호 등으로 구성되며, 각 버전 번호의 부분은 점 (`.`) 으로 구분된다. 비교 연산자와 버전 번호 사이에는 공백이 있어서는 안 된다. `compiler()`를 위한 버전은 컴파일러 버전이며, 컴파일러에 전달되는 Swift 버전 설정과는 관계가 없다. `swift()`를 위한 버전은 현재 컴파일되는 언어의 버전이다. 예를 들어, Swift 4.2 모드에서 Swift 5 컴파일러를 사용하여 코드를 컴파일한다면, 컴파일러 버전은 5이며 언어 버전은 4.2다. 이러한 설정과 함께, 다음의 코드는 총 세 개의 메세지를 출력한다.

```swift
#if compiler(>=5)
print("Compiled with the Swift 5 compiler or later")
#endif
#if swift(>=4.2)
print("Compiled in Swift 4.2 mode or later")
#endif
#if compiler(>=5) && swift(<5)
print("Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5")
#endif
// Prints "Compiled with the Swift 5 compiler or later"
// Prints "Compiled in Swift 4.2 mode or later"
// Prints "Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5"
```

`canImport()` 플랫폼 조건을 위한 인자는 모든 플랫폼에서 나타나지 않을 수 있는 모듈의 이름이다. 이 조건은 모듈을 임포트할 수 있는지를 테스트하지만, 실제로 임포트하지는 않는다. 모듈이 존재한다면, 플랫폼 조건은 `true`를 반환한다; 그렇지 않으면, `false`를 반환한다.

`targetEnvironment()` 플랫폼 조건은 코드가 시뮬레이터를 위해 컴파일된다면 `true`를 반환한다; 그렇지 않으면, `false`를 반환한다.

- 알아두기

  `arch(arm)` 플랫폼 조건은 ARM 64 디바이스에 대해 `true`를 반환하지 않는다. `arch(i386)` 플랫폼 조건은 코드가 32비트 iOS 시뮬레이터를 위해 코드가 컴파일될 때 `true`를 반환한다.

논리 연산자 `&&`, `||`, `!`를 사용하고 그룹 지정을 위해 소괄호를 사용하여 컴파일 조건을 조합할 수 있다. 이러한 연산자들은 평범한 불리언 표현식을 조합하기 위해 사용되는 논리 연산자들처럼 같은 연관성*associativity*과 우선순위를 갖는다.

`if` 구문과 비슷하게, 다른 컴파일 조건에 대해 테스트하기 위해 여러 개의 조건 분기를 추가할 수 있다. `#elseif`절을 사용하여 추가적인 분기를 몇 개든 추가할 수 있다. 또한 `#else`절을 사용하여 마지막 추가 분기를 추가할 수 있다. 여러 개의 분기를 포함하는 조건적 컴파일 블록은 다음의 형식을 갖는다.

```swift
#if {compilation condition 1}
{statements to compile if compilation condition 1 is true}
#elseif {compilation condition 2}
{statements to compile if compilation condition 2 is true}
#else
{statements to compile if both compilation conditions are false}
#endif
```

- 알아두기

  조건적 컴파일 블록의 구현부에 있는 각각의 구문은 컴파일되지 않더라도 파싱된다. 그러나 컴파일 조건이 `swift()` 플랫폼 조건을 포함한다면 예외가 있다: 해당 구문은 오직 Swift의 컴파일러 버전이 플랫폼 조건에 지정된 것과 일치할 때만 파싱된다. 이 예외는 오래된 컴파일러가 Swift의 새로운 버전에서 도입된 신택스를 파싱하는 것을 시도하지 않는다는 것을 보장한다.

### 라인 제어 구문

라인 제어 구문은 컴파일되고 있는 소스 코드의 라인 번호 및 파일 이름과 다를 수 있는 라인 번호 및 파일 이름을 지정하기 위해 사용된다. 라인 컨트롤 구문을 사용하여 Swift가 진단 및 디버깅 프로세스를 위해 사용하는 소스 코드의 위치를 변경할 수 있다.

라인 제어 구문은 다음의 형식을 갖는다.

```swift
#sourceLocation(file: {file path}, line: {line number})
#sourceLocation()
```

 첫 번째 라인 제어 구문의 형식은 `#line`, `#file`, `#filePath` 리터럴 표현식들의 값을 변경하며, 코드의 라인으로 시작하여 라인 제어 구문이 뒤따른다. *line* *number*는 `#line`의 값을 변경하고 0보다 큰 정수 리터럴이 된다. *file path*는 `#file`과 `#filePath`의 값을 변경하며, 문자열 리터럴이 된다. 지정된 문자열은 `#filePath`의 값이 되며, 문자열의 마지막 경로 컴포넌트는 `#file`의 값이 된다.

두 번째 라인 제어 구문의 형식 `#sourceLocation()`은 소스 코드의 위치를 기본 라인 번호와 파일 경로로 재설정한다.

### 컴파일 타임 진단 구문

컴파일 타임 진단 구문은 컴파일러가 컴파일하는 동안 에러나 경고를 낼 수 있게 한다. 컴파일 타임 진단 구문은 다음의 형식을 갖는다.

```swift
#error("{error message}")
#warning("{warning message}")
```

첫 번째 형식은 치명적인 에러로 *error message*를 내고 컴파일 프로세스를 종료한다. 두 번째 형식은 치명적이지 않은 경고로 *warning message*를 내고 컴파일이 계속되도록 한다. 정적 문자열 리터럴로 진단 메세지를 작성한다. 정적 문자열 리터럴은 문자열 보간법이나 이어붙이기 같은 기능을 사용할 수 없으나, 여러 줄 문자열 리터럴 신택스는 사용할 수 있다.

## 이용 가능성 조건

*이용 가능성 조건availability condition*은 지정된 플랫폼 인자에 기반하여 런타임에서 API의 이용 가능성을 질의하기 위해 `if`, `while`, `guard` 구문의 조건으로서 사용된다.

이용 가능성 조건은 다음의 형식을 갖는다.

```swift
if #available({platform name} {version}, {...}, *) {
    {statements to execute if the APIs are available}
} else {
    {fallback statements to execute if the APIs are unavailable}
}
```

사용하려는 API가 런타임에서 이용 가능한지에 따라 코드 블록을 실행하기 위해 이용 가능성 조건을 사용한다. 컴파일러는 해당 코드 블록에 있는 API가 이용 가능한지를 검증할 때 이용 가능성 조건으로부터의 정보를 사용한다.

이용 가능성 조건은 콤마로 구분된 플랫폼 이름과 버전의 리스트를 취한다. 플랫폼 이름으로 `iOS`, `macOS`, `watchOS`, `tvOS`를 사용하고, 상응하는 버전 번호를 포함한다. `*` 인자는 필수적이며, 다른 플랫폼에서 이용 가능성 조건에 의해 보호된 코드 블록의 구현부가 당신의 타겟이 지정한 최소 배포 타겟에서 실행되도록 지정한다.

불리언 조건과는 다르게, `&&`와 `||`와 같은 논리 연산자를 사용하여 이용 가능성 조건을 조합할 수 없다.