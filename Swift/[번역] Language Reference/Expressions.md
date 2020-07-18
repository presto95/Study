# 표현식

Swift에는 네 가지 종류의 표현식이 있다: 전위*prefix* 표현식, 이항*binary* 표현식, 주요*primary* 표현식, 후위*postfix* 표현식. 표현식을 평가하는 것은 값을 반환하거나, 사이드 이펙트를 발생시거나, 또는 두 개 모두 발생시킨다.

전위 및 이항 표현식은 더 작은 표현식에 연산자를 적용할 수 있게 해준다. 주요 표현식은 개념적으로 가장 간단한 종류의 표현식이고, 값들에 접근할 수 있는 방법을 제공한다. 전위 및 이항 표현식처럼, 후위 표현식은 함수 호출과 멤버 접근과 같은 접미어들을 사용하여 더욱 복잡한 표현식을 만들 수 있게 해준다. 각 종류의 표현식들은 아래 섹션에서 더 자세하게 기술된다.

## 전위 표현식

*전위 표현식*은 표현식에 선택적인 전위 연산자를 조합한다. 전위 연산자는 하나의 인자를 취하고, 표현식은 그 다음에 위치한다.

표준 라이브러리의 연산자 이외에도, 변수의 이름 바로 앞에 `&`를 붙여서 함수 호출 표현식에 인-아웃 인자로 넘겨줄 수 있다.

### Try 연산자

*try 표현식*은 `try` 연산자 뒤에 에러를 던질 수 있는 표현식이 오는 것으로 구성된다. 다음의 형식을 갖는다.

```swift
try {expression}
```

*optional-try 표현식*은 `try?` 연산자 뒤에 에러를 던질 수 있는 표현식이 오는 것으로 구성된다. 다음의 형식을 갖는다.

```swift
try? {expression}
```

*expression*이 에러를 던지지 않는다면, optinal-try 표현식의 값은 *expression*의 값을 담는 옵셔널이 된다. 그렇지 않으면, optional-try 표현식의 값은 `nil`이 된다.

*forced-try 표현식*은 `try!` 연산자 뒤에 에러를 던질 수 있는 표현식이 오는 것으로 구성된다. 다음의 형식을 갖는다.

```swift
try! {expression}
```

*expression*이 에러를 던진다면, 런타임 에러가 발생한다.

이항 연산자의 왼쪽에 있는 표현식이 `try`, `try?`, `try!`로 표시되었을 때, 이 연산자는 모든 이항 표현식에 대해 적용된다. 즉, 연산자 적용 스코프를 명시적으로 하기 위해 소괄호를 사용할 수 있다.

```swift
sum = try someThrowingFunction() + anotherThrowingFunction()   // try applies to both function calls
sum = try (someThrowingFunction() + anotherThrowingFunction()) // try applies to both function calls
sum = (try someThrowingFunction()) + anotherThrowingFunction() // Error: try applies only to the first function call
```

`try` 표현식은 이항 연산자가 할당 연산자이거나 `try` 표현식이 소괄호에 감싸져 있지 않다면, 이항 연산자의 오른쪽에 나타날 수 없다.

## 이항 표현식

*이항 연산자*는 왼쪽 인자와 오른쪽 인자를 취하는 표현식과 함께 중위 이항 연산자를 조합한다. 다음의 형식을 갖는다.

```swift
{left-hand argument} {operator} {right-hand argument}
```

- 알아두기

  파싱 타임에서, 이항 연산자로 만들어진 표현식은 평평한 리스트로 표현된다. 이 리스트는 연산자 우선순위를 적용하여 트리로 변형된다. 예를 들어, `2 + 3 * 5` 표현식은 초기에 다섯 개의 아이템 `2`, `+`, `3`, `*`, `5`를 가진 평평한 리스트로 이해된다. 이 프로세스는 그것을 (2 + (3 * 5)) 트리로 변환한다.

### 할당 연산자

*할당 연산자*는 주어진 표현식에 새로운 값을 설정한다. 다음의 형식을 갖는다.

```swift
{expression} = {value}
```

*expression*의 값은 *value*를 평가하여 획득한 값으로 설정된다. *expression*이 튜플이면, *value*는 반드시 같은 요소의 개수를 가진 튜플이어야 한다. (중첩 튜플은 허용된다.) 할당은 *value*의 각 부분에서 상응하는 *expression*의 각 부분으로 수행된다.

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a is "test", b is 12, c is 3, and 9.45 is ignored
```

할당 연산자는 어떠한 값도 반환하지 않는다.

### 삼항 조건 연산자

*삼항 조건 연산자*는 조건식의 값에 기반하여 두 개의 주어진 값들 중 하나를 평가한다. 다음의 형식을 갖는다.

```swift
{condition} ? {expression used if true} : {expression used if false}
```

*condition*이 `true`로 평가되면, 조건 연산자는 첫 번째 표현식을 평가하고 그 값을 반환한다. 그렇지 않으면, 두 번째 표현식을 평가하고 그 값을 반환한다. 사용되지 않은 표현식은 평가되지 않는다.

### 타입 캐스팅 연산자

네 개의 타입 캐스팅 연산자가 있다: `is` 연산자, `as` 연산자, `as?` 연산자, 그리고 `as!` 연산자다.

다음의 형식을 갖는다.

```swift
{expression} is {type}
{expression} as {type}
{expression} as? {type}
{expression} as! {type}
```

`is` 연산자는 *expression*이 지정된 *type*으로 캐스팅될 수 있는지를 런타임에서 확인한다. *expression*이 지정된 *type*으로 캐스팅될 수 있다면 `true`를 반환한다; 그렇지 않으면, `false`를 반환한다.

`as` 연산자는 컴파일 타임에서 캐스팅이 항상 성공한다는 것을 알 때, 즉 업캐스팅이나 브릿징 같은 것일 때 캐스팅을 수행한다. 업캐스팅은 중개 역할을 하는 변수를 사용하지 않고도, 표현식을 그 타입의 상위 타입의 인스턴스인 것처럼 사용하게 해준다. 다음의 접근법들은 동일하다.

```swift
func f(_ any: Any) { print("Function for Any") }
func f(_ int: Int) { print("Function for Int") }
let x = 10
f(x)
// Prints "Function for Int"

let y: Any = x
f(y)
// Prints "Function for Any"

f(x as Any)
// Prints "Function for Any"
```

브릿징은 `String`과 같은 Swift 표준 라이브러리의 타입의 표현식이 새로운 인스턴스를 생성할 필요 없이 `NSString`과 같이 상응하는 Foundation 타입을 사용할 수 있게 해준다.

`as?` 연산자는 *expression*을 지정된 *type*으로 조건적 캐스팅을 수행한다. `as?` 연산자는 지정된 *type*의 옵셔널을 반환한다. 런타임에서 캐스팅이 성공하면, *expression*의 값은 옵셔널에 래핑되어 반환된다; 그렇지 않으면, 반환된 값은 `nil`이 된다. 지정된 *type*으로의 캐스팅이 실패하는 것을 보장하거나 성공하는 것을 보장한다면, 컴파일 타임 에러가 발생한다.

`as!` 연산자는 *expression*을 지정된 *type*으로 강제 캐스팅을 수행한다. `as!` 연산자는 지정된 *type*의 옵셔널 타입이 아니라, 값을 반환한다. 캐스팅이 실패하면 런타임 에러가 발생한다. `x as! T`의 동작은 `(x as? T)!`의 동작과 같다.

## 주요 표현식

*주요 표현식*은 표현식의 가장 기본적인 종류다. 이들은 그 자체로 표현식으로 사용될 수 있고, 전위 표현식, 이항 표현식, 후위 표현식을 만들기 위해 다른 토큰들과 혼합될 수 있다.

### 리터럴 표현식

*리터럴 표현식*은 (문자열이나 숫자 같은) 평범한 리터럴, 배열 또는 딕셔너리 리터럴, 플레이그라운드 리터럴, 또는 다음의 특별한 리터럴 중 하나로 구성된다.

[특별한 리터럴](https://www.notion.so/3af0273b201342c3ac45d199be0d1e96)

`#file` 표현식의 문자열 값은 *module/file*의 형식을 가지며, *file*은 표현식이 나타나는 파일의 이름이고 *module*은 이 파일이 위치하는 모듈의 이름이다. `#filePath` 표현식의 문자열 값은 표현식이 나타나는 파일에 대한 완전한 파일 시스템 경로다. 이 두 개의 값들 모두 `#sourceLocation`에 의해 변경될 수 있다.

- 알아두기

  `#file` 표현식을 파싱할 때, 텍스트에서 첫 번째 슬래시 (`/`) 앞의 것을 모듈 이름이고, 마지막 슬래시 뒤에 있는 것을 파일 이름으로 읽어라. 앞으로 이 문자열은 `MyModule/some/disambiguation/MyFile.swift`처럼 여러 개의 슬래시를 포함할 수 있다.

함수 안에서, `#function`의 값은 해당 함수의 이름이며, 메소드 안에서 이는 해당 메소드의 이름이며, 프로퍼티 접근자 또는 설정자에서 이는 해당 프로퍼티의 이름이며, `init`이나 `subscript`와 같은 특별한 멤버 안에서 이는 해당 키워드의 이름이며, 파일의 최상위 수준에서 이는 현재 모듈의 이름이다.

이들이 함수나 메소드 매개변수의 기본값으로 사용될 때, 특별한 리터럴의 값은 기본값 표현식이 호출 시점에서 평가될 때 결정된다.

```swift
func logFunctionName(string: String = #function) {
    print(string)
}
func myFunction() {
    logFunctionName() // Prints "myFunction()".
}
```

*배열 리터럴*은 값들의 순서 있는 컬렉션이다. 다음의 형식을 갖는다.

```swift
[{value 1}, {value 2}, {...}]
```

배열에 있는 마지막 표현식은 선택적으로 콤마를 뒤에 붙일 수 있다. 배열 리터럴의 값은 `[T]` 타입을 가지며, `T`는 그 안에 있는 표현식의 타입이다. 여러 개의 타입을 갖는 표현식들이 있다면, `T`는 그것들의 가장 가까운 공통 상위 타입이 된다. 빈 배열 리터럴은 빈 대괄호 쌍을 사용하여 작성되며, 지정된 타입의 빈 배열을 만들기 위해 사용될 수 있다.

```swift
var emptyArray: [Double] = []
```

*딕셔너리 리터럴*은 키-값 쌍의 순서 없는 컬렉션이다. 다음의 형식을 갖는다.

```swift
[{key 1}: {value 1}, {key 2}: {value 2}, {...}]
```

딕셔너리에 있는 마지막 표현식은 선택적으로 콤마를 뒤에 붙일 수 있다. 딕셔너리 리터럴의 값은 `[Key: Value]` 타입을 가지며, `Key`는 키 표현식의 타입이고 `Value`는 값 표현식의 타입이다. 여러 개의 타입을 갖는 표현식들이 있다면, `Key`와 `Value`는 각각의 값들에 대하여 가장 가까운 공통 상위 타입이 된다. 빈 딕셔너리 리터럴은 빈 배열 리터럴과 구분하기 위해 대괄호 쌍 안에 콜론을 넣어 (`[:]`) 작성된다. 빈 딕셔너리 리터럴을 사용하여 지정된 키와 값 타입의 빈 딕셔너리를 만들 수 있다.

```swift
var emptyDictionary: [String: Double] = [:]
```

*플레이그라운드 리터럴*은 프로그램 에디터 안에서 상호 작용하는 색상, 파일, 또는 이미지의 표현을 만들기 위해 Xcode가 사용하는 것이다. Xcode 바깥에서 플레인 텍스트로 있는 플레이그라운드 리터럴은 특별한 리터럴 신택스를 사용하여 표현된다.

- `#colorLiteral`, `#fileLiteral`, `#imageLiteral` 같은 것들

### 셀프 표현식

`self` 표현식은 현재 타입 또는 해당 타입의 인스턴스에 대한 명시적 참조이다. 다음의 형식을 갖는다.

```swift
self
self.{member name}
self[{subscript index}]
self({initializer arguments})
self.init({initializer arguments})
```

이니셜라이저, 서브스클비트, 또는 인스턴스 메소드 안에서, `self`는 그것이 발생하는 타입의 현재 인스턴스를 참조한다. 타입 메소드에서 `self`는 그것이 발생하는 현재 타입을 참조한다.

`self` 표현식은 멤버에 접근할 때 스코프를 지정하기 위해 사용되며, 이는 함수 매개변수와 같이 스코프 내에 같은 이름을 가진 또다른 변수가 있을 때 모호성을 줄일 수 있게 해준다.

```swift
class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}
```

값 타입에 대해 가변*mutating* 메소드에서, `self.`에 새로운 해당 값 타입의 인스턴스를 할당할 수 있다.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

### 슈퍼클래스 표현식

*슈퍼클래스 표현식*은 클래스가 그것의 슈퍼클래스와 상호작용할 수 있게 해준다. 다음의 형식 중 하나를 갖는다.

```swift
super.{member name}
super[{subscript index}]
super.init({initializer arguments})
```

첫 번째 형식은 슈퍼클래스의 멤버에 접근하기 위해 사용된다. 두 번째 형식은 슈퍼클래스의 서브스크립트 구현에 접근하기 위해 사용된다. 세 번째 형식은 슈퍼클래스의 이니셜라이저에 접근하기 위해 사용된다.

서브클래스는 멤버, 서브스크립트, 이니셜라이저 구현에서 슈퍼클래스 표현식을 사용하여 슈퍼클래스에서의 구현을 사용할 수 있다.

### 클로저 표현식

*클로저 표현식*은 다른 프로그래밍 언어에서 *람다*나 *익명 함수*로도 알려진 클로저를 만든다. 함수 선언처럼, 클로저는 구문을 포함하며, 그것이 감싸는 스코프에서 상수와 변수를 획득한다. 다음의 형식을 갖는다.

```swift
{ ({parameters}) -> {return type} in
    {statements}
}
```

*parameters*는 함수 선언에서의 매개변수들과 같은 형식을 갖는다.

클로저를 더욱 간결하게 작성할 수 있게 해주는 몇 가지 특별한 형식이 있다.

- 클로저는 그 매개변수들의 타입과 반환형, 또는 둘 모두를 생략할 수 있다. 매개변수 이름과 타입을 모두 생략한다면, 구문 이전에 `in` 키워드를 생략한다. 생략된 타입이 추론될 수 없다면 컴파일 타임 에러가 발생한다.
- 클로저는 매개변수들의 이름을 생략할 수 있다. 그러면 그 매개변수들은 암시적으로  `$` 다음에 그것들의 위치가 오는 이름을 갖게 된다: `$0`, `$1`, `$2`, 등등.
- 오직 하나의 표현식으로 구성된 클로저는 해당 표현식의 값을 반환하는 것으로 이해된다. 이 표현식의 컨텐츠는 또한 주변 표현식에서 타입 추론을 수행할 때 고려된다.

다음의 클로저 표현식들은 동일하다.

```swift
myFunction { (x: Int, y: Int) -> Int in
    return x + y
}

myFunction { x, y in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

클로저 표현식은 함수 호출의 일부분으로 클로저를 즉시 사용할 때처럼, 변수나 상수에 저장되지 않고도 사용될 수 있다. 위의 코드에서 `myFunction`에 넘겨진 클로저 표현식은 이러한 즉시 사용의 예시들이다. 결과적으로 클로저 표현식은 표현식의 주변 컨텍스트에 따라 탈출 처리되거나 탈출 처리되지 않는다. 클로저 표현식은 즉시 호출되거나 탈출 처리되지 않은 함수의 인자로 넘겨진다면 탈출 처리되지 않는다. 그렇지 않으면 클로저 표현식은 탈출 처리된다.

**획득 리스트**

기본적으로, 클로저 표현식은 주변 스코프에서 상수들과 변수들을 획득하며, 이것들에 강한 참조를 한다. *획득 리스트*를 사용하여 클로저에서 값들이 어떻게 획득되는지에 대해 명시적으로 제어할 수 있다.

획득 리스트는 매개변수 리스트의 이전에서 대괄호로 둘러싸여 콤마로 구분된 표현식의 리스트로 작성된다. 획득 리스트를 사용한다면, 반드시 `in` 키워드도 사용해야 한다. 심지어 매개변수 이름, 매개변수 타입, 반환형을 생략했더라도 말이다.

획득 리스트에 있는 요소들은 클로저가 만들어질 때 초기화된다. 획득 리스트의 각각의 요소에 대하여, 상수는 주변 스코프에서 같은 이름을 가진 상수나 변수의 값으로 초기화된다. 예를 들어, 아래의 코드에서 `a`는 획득 리스트에 포함되지만 `b`는 그렇지 않아서, 다른 동작을 하고 있다.

```swift
var a = 0
var b = 0
let closure = { [a] in
    print(a, b)
}

a = 10
b = 10
closure()
// Prints "0 10"
```

`a`라고 이름 지어진 두 개의 다른 것들이 있다. 하나는 주변 스코프에 있는 변수고, 다른 하나는 클로저 스코프에 있는 상수다. 하지만 `b`라고 이름 지어진 변수는 오직 하나다. 안쪽 스코프에 있는 `a`는 클로저가 만들어질 때 바깥쪽 스코프에 있는 `a`의 값으로 초기화되지만, 그것들의 값은 어떠한 특별한 방법으로도 연결되지 않는다. 이는 바깥쪽 스코프에 있는 `a`의 값에 대한 변화가 안쪽 스코프에 있는 `a`의 값에 영향을 주지 않으며, 또한 클로저 안에 있는 `a`의 변화도 클로저 바깥에 있는 `a`의 값에 영향을 주지 않는다. 대조적으로, `b`라고 이름 지어진 것은 오직 하나-바깥쪽 스코프에 있는 `b`-이므로, 클로저 안쪽 또는 바깥쪽에서의 변화는 두 곳 모두에서 나타난다.

이러한 차이는 획득한 변수의 타입이 레퍼런스 시맨틱스일 때는 나타나지 않는다. 예를 들어, 아래의 코드에서 `x`라고 이름 지어진 것은 두 개가 있으며, 하나는 바깥쪽 스코프에 있는 변수이고 다른 하나는 안쪽 스코프에 있는 상수다. 하지만 이 두 개는 레퍼런스 시맨틱스에 있기 때문에 같은 객체를 참조한다.

```swift
class SimpleClass {
    var value: Int = 0
}
var x = SimpleClass()
var y = SimpleClass()
let closure = { [x] in
    print(x.value, y.value)
}

x.value = 10
y.value = 10
closure()
// Prints "10 10"
```

표현식의 값의 타입이 클래스라면, 획득 리스트에서 그 표현식을 `weak`나 `unowned`로 표시하여 그 표현식의 값에 대해 약한 참조 또는 미소유 참조로 설정할 수 있다.

```swift
myFunction { print(self.title) }                    // implicit strong capture
myFunction { [self] in print(self.title) }          // explicit strong capture
myFunction { [weak self] in print(self!.title) }    // weak capture
myFunction { [unowned self] in print(self.title) }  // unowned capture
```

또한 획득 리스트에서 이름 있는 값에 대해 임의의 표현식을 바인딩할 수 있다. 그 표현식은 클로저가 만들어질 때 평가되고, 그 값은 지정된 강도로 획득된다.

```swift
// Weak capture of "self.parent" as "parent"
myFunction { [weak parent = self.parent] in print(parent!.title) }
```

### 암시적인 멤버 표현식

*암시적인 멤버 표현식*은 열거형의 케이스나 타입 메소드처럼, 타입 추론이 암시된 타입을 결정할 수 있는 컨텍스트에서 타입의 멤버에 접근하기 위한 축약된 방식이다. 다음의 형식을 갖는다.

```swift
.{member name}
```

```swift
var x = MyEnumeration.someValue
x = .anotherValue
```

### 소괄호로 둘러싸여진 표현식

*소괄호로 둘러싸여진 표현식*은 소괄호로 둘러싸여진 표현식으로 구성된다. 명시적으로 표현식을 그룹 지정하여 연산자들의 우선순위를 지정하기 위해 소괄호를 사용할 수 있다. 그룹 지정된 소괄호들은 표현식의 타입을 변경하지 않는다. 예를 들어, `(1)`의 타입은 단순히 `Int`가 된다.

### 튜플 표현식

*튜플 표현식*은 소괄호로 둘러써야져 콤마로 구분된 표현식의 리스트로 구성된다. 각각의 표현식은 그 이전에 선택적으로 식별자를 가질 수 있고, 콜론 (`:`)으로 구분된다. 다음의 형식을 갖는다.

```swift
({identifier 1}: {expression 1}, {identifier 2}: {expression 2}, {...})
```

튜플 표현식에서 각각의 식별자는 튜플 표현식의 스코프에서 반드시 유일해야 한다. 중첩 튜플 표현식에서, 같은 중첩 수준에 있는 식별자들은 반드시 유일해야 한다. 예를 들어, `(a: 10, a: 20)`은 `a` 레이블이 같은 수준에서 두 번 나타났으므로 유효하지 않다. 그러나 `(a: 10, b: (a: 1, x: 2))`는 `a`가 두 번 나타났을지라도 유효하다. 바깥쪽 튜플에서 한 번, 안쪽 튜플에서 한 번 나타났기 때문이다.

튜플 표현식은 0개의 표현식, 또는 두 개 이상의 표현식을 포함할 수 있다. 소괄호 안에 있는 하나의 표현식은 소괄호로 둘러싸여진 표현식이다.

- 알아두기

  빈 튜플 표현식과 빈 튜플 타입 모두 Swift에서 `()`로 작성된다. `Void`는 `()`의 타입 별칭이기 때문에, 이를 사용하여 빈 튜플 타입을 작성할 수 있다. 그러나, 모든 타입 별칭들처럼, `Void`는 항상 타입이다-이를 사용하여 빈 튜플 표현식을 작성할 수 없다.

### 와일드카드 표현식

*와일드카드 표현식*은 할당 중에 값을 명시적으로 무시하기 위해 사용된다. 예를 들어, 다음의 할당에서 10은 `x`에 할당되고, 20은 무시된다.

```swift
(x, _) = (10, 20)
// x is 10, and 20 is ignored
```

### 키-경로 표현식

*키-경로 표현식key-path expression*은 타입의 프로퍼티나 서브스크립트를 참조한다. 키-경로 표현식을 사용하여 키-값 옵저빙과 같은 동적인 프로그래밍 태스크를 할 수 있다. 다음의 형식을 갖는다.

```swift
\{type name}.{path}
```

*type name*은 `String`, `[Int]`, 또는 `Set<Int>`처럼 구체 타입의 이름이며, 어떠한 제네릭 매개변수를 포함할 수 있다.

*path*는 프로퍼티 이름, 서브스크립트, 옵셔널 체이닝 표현식, 강제 언래핑 표현식으로 구성된다. 이러한 키-경로 컴포넌트들 각각은 어떠한 순서로든 필요한 만큼 반복될 수 있다.

컴파일 타임에서 키-경로 표현식은 `KeyPath` 클래스의 인스턴스로 교체된다.

키 경로를 사용하여 값에 접근하기 위해 `subscript(keyPath:)` 서브스크립트에 키 경로를 넘겨라. 이는 모든 타입에서 사용 가능하다.

```swift
struct SomeStructure {
    var someValue: Int
}

let s = SomeStructure(someValue: 12)
let pathToProperty = \SomeStructure.someValue

let value = s[keyPath: pathToProperty]
// value is 12
```

*type name*은 타입 추론이 암시된 타입을 결정하는 컨텍스트에서 생략될 수 있다. 다음의 코드는 `\SomeClass.someProperty` 대신 `\.someProperty`를 사용한다.

```swift
class SomeClass: NSObject {
    @objc dynamic var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 10)
c.observe(\.someProperty) { object, change in
    // ...
}
```

*path*는 아이덴티티 키 경로 (`\.self)`를 만들기 위해 `self`를 참조한다. 아이덴티티 키 경로는 전체 인스턴스를 참조하므로, 하나의 단계에서 변수에 저장된 모든 데이터에 대해 접근하고 변경하기 위해 사용할 수 있다.

```swift
var compoundValue = (a: 1, b: 2)
// Equivalent to compoundValue = (a: 10, b: 20)
compoundValue[keyPath: \.self] = (a: 10, b: 20)
```

*path*는 마침표로 구분된 여러 개의 프로퍼티 이름을 포함하여 르포러티의 값의 프로퍼티를 참조할 수 있다. 이 코드는 키 경로 표현식 `\OuterStructure.outer.someValue`를 사용하여 `OuterStructure` 타입의 `outer` 프로퍼티의 `someValue` 프로퍼티에 접근한다.

```swift
struct OuterStructure {
    var outer: SomeStructure
    init(someValue: Int) {
        self.outer = SomeStructure(someValue: someValue)
    }
}

let nested = OuterStructure(someValue: 24)
let nestedKeyPath = \OuterStructure.outer.someValue

let nestedValue = nested[keyPath: nestedKeyPath]
// nestedValue is 24
```

*path*는 대괄호를 사용한 서브스크립트를 포함할 수 있는데, 이 때 서브스크립트의 매개변수 타입은 `Hashable` 프로토콜을 준수해야 한다. 이 예제는 배열의 두 번째 요소에 접근하기 위해 키 경로에서 서브스크립트를 사용한다.

```swift
let greetings = ["hello", "hola", "bonjour", "안녕"]
let myGreeting = greetings[keyPath: \[String].[1]]
// myGreeting is 'hola'
```

서브스크립트에서 사용된 값은 이름 있는 값이거나 리터럴일 수 있다. 값들은 값 시맨틱스를 사용하여 키 경로에서 획득된다. 다음의 코드는 키-경로 표현식과 클로저에서 `index` 변수를 사용하여 `greetings` 배열의 세 번째 요소에 접근한다. `index`가 수정될 때, 키-경로 표현식은 여전히 세 번째 요소를 참조하는 반면, 클로저는 새로운 인덱스를 사용한다.

```swift
var index = 2
let path = \[String].[index]
let fn: ([String]) -> String = { strings in strings[index] }

print(greetings[keyPath: path])
// Prints "bonjour"
print(fn(greetings))
// Prints "bonjour"

// Setting 'index' to a new value doesn't affect 'path'
index += 1
print(greetings[keyPath: path])
// Prints "bonjour"

// Because 'fn' closes over 'index', it uses the new value
print(fn(greetings))
// Prints "안녕"
```

*path*는 옵셔널 체이닝과 강체 언래핑을 사용할 수 있다. 이 코드는 키 경로에서 옵셔널 체이닝을 사용하여 옵셔널 문자열의 프로퍼티에 접근한다.

```swift
let firstGreeting: String? = greetings.first
print(firstGreeting?.count as Any)
// Prints "Optional(5)"

// Do the same thing using a key path.
let count = greetings[keyPath: \[String].first?.count]
print(count as Any)
// Prints "Optional(5)"
```

키 경로들의 컴포넌트들을 짜맞추어 타입에 깊게 중첩된 값에 접근할 수 있다. 다음의 코드는 키-경로 표현식들을 자합하여 배열들의 딕셔너리의 값들과 프로퍼티들에 접근한다.

```swift
let interestingNumbers = ["prime": [2, 3, 5, 7, 11, 13, 17],
                          "triangular": [1, 3, 6, 10, 15, 21, 28],
                          "hexagonal": [1, 6, 15, 28, 45, 66, 91]]
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]] as Any)
// Prints "Optional([2, 3, 5, 7, 11, 13, 17])"
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]![0]])
// Prints "2"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count])
// Prints "7"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count.bitWidth])
// Prints "64"
```

보통 함수나 클로저를 제공할 만한 컨텍스트에 키 경로 표현식을 사용할 수 있다. 특히 루트 타입이 `SomeType`이고 그것의 경로가 `(SomeType) -> Value` 같은 함수나 클로저 타입이 아닌 `Value` 타입의 값을 만들어낼 때 키 경로 표현식을 사용할 수 있다.

```swift
struct Task {
    var description: String
    var completed: Bool
}
var toDoList = [
    Task(description: "Practice ping-pong.", completed: false),
    Task(description: "Buy a pirate costume.", completed: true),
    Task(description: "Visit Boston in the Fall.", completed: false),
]

// Both approaches below are equivalent.
let descriptions = toDoList.filter(\.completed).map(\.description)
let descriptions2 = toDoList.filter { $0.completed }.map { $0.description }
```

키 경로 표현식의 어떠한 사이드 이펙트들은 표현식이 평가되는 지점에서만 평가된다. 예를 들어, 키 경로 표현식에서 서브스크립트 안에서 함수 호출을 한다면, 그 함수는 오직 표현식 평가의 일부분으로서 오직 한 번만 호출되고, 키 경로가 사용되는 모든 경우에 호출되지 않는다.

```swift
func makeIndex() -> Int {
    print("Made an index")
    return 0
}
// The line below calls makeIndex().
let taskKeyPath = \[Task][makeIndex()]
// Prints "Made an index"

// Using taskKeyPath doesn't call makeIndex() again.
let someTask = toDoList[keyPath: taskKeyPath]
```

### 셀렉터 표현식

셀렉터 표현식은 Objective-C에서 메소드나 프로퍼티의 접근자 또는 설정자를 참조하기 위해 사용되는 셀렉터에 접근할 수 있게 해준다. 다음의 형식을 갖는다.

```swift
#selector({method name})
#selector(getter: {property name})
#selector(setter: {property name})
```

*method name*과 *property name*은 반드시 Objective-C 런타임에서 사용 가능한 메소드나 프로퍼티에 대한 참조여야 한다. 셀렉터 표현식의 값은 `Selector` 타입의 인스턴스다.

```swift
class SomeClass: NSObject {
    @objc let property: String

    @objc(doSomethingWithInt:)
    func doSomething(_ x: Int) { }

    init(property: String) {
        self.property = property
    }
}
let selectorForMethod = #selector(SomeClass.doSomething(_:))
let selectorForPropertyGetter = #selector(getter: SomeClass.property)
```

프로퍼티의 접근자에 대한 셀렉터를 만든다면, *property name*은 변수나 상수 프로퍼티에 대한 참조일 수 있다. 대조적으로, 프로퍼티의 설정자에 대한 셀렉터를 만든다면, *property name*은 반드시 변수 프로퍼티에 대한 참조여야만 한다.

*method name*은 그룹 지정을 위해 소괄호에 감싸질 수 있으며, 또한 `as` 연산자는 이름을 공유하지만 다른 타입 시그니쳐를 가진 메소드 간의 모호성을 없애기 위해 사용될 수 있다.

```swift
extension SomeClass {
    @objc(doSomethingWithString:)
    func doSomething(_ x: String) { }
}
let anotherSelector = #selector(SomeClass.doSomething(_:) as (SomeClass) -> (String) -> Void)
```

셀렉터는 런타임이 아닌 컴파일 타임에 만들어지기 때문에, 컴파일러는 메소드나 프로퍼티가 존재하고 그것들이 Objective-C 런타임에 노출되어 있는지 확인할 수 있다.

- 알아두기

  *method name*과 *property name*이 표현식일지라도, 이들은 절대 평가되지 않는다.

### 키-경로 문자열 표현식

키-경로 문자열 표현식은 Objective-C에 있는 프로퍼티를 참조하기 위해 사용되는 문자열에 접근할 수 있게 해주며, 이는 키-값 코딩과 키-값 옵저빙 API에서 사용된다. 다음의 형식을 갖는다.

```swift
#keyPath({property name})
```

*property name*은 반드시 Objective-C 런타임에서 사용 가능한 프로퍼티를 참조해야 한다. 컴파일 타임에서 키-경로 문자열 표현식은 문자열 리터럴로 교체된다.

```swift
class SomeClass: NSObject {
    @objc var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 12)
let keyPath = #keyPath(SomeClass.someProperty)

if let value = c.value(forKey: keyPath) {
    print(value)
}
// Prints "12"
```

클래스 안에서 키-경로 문자열 표현식을 사용한다면, 클래스 이름 없이 프로퍼티 이름만 작성하여 클래스의 프로퍼티를 참조할 수 있다.

```swift
extension SomeClass {
    func getSomeKeyPath() -> String {
        return #keyPath(someProperty)
    }
}
print(keyPath == c.getSomeKeyPath())
// Prints "true"
```

키 경로 문자열은 런타임이 아니라 컴파일 타임에서 만들어지므로, 컴파일러는 프로퍼티가 존재하고 Objective-C 런타임에 노출되어 있는지를 확인할 수 있다.

- 알아두기

  *property name*이 표현식일지라도, 절대 평가되지 않는다.

## 후위 표현식

*후위 표현식*은 표현식에 후위 연산자나 다른 접미어 신택스를 붙여서 형성된다. 구문적으로, 모든 주요 표현식은 또한 후위 표현식이다.

### 함수 호출 표현식

*함수 호출 표현식*은 함수 이름 뒤에 소괄호로 둘러싸인 콤마로 구분된 함수의 인자 리스트로 구성된다. 함수 호출 표현식은 다음의 형식을 갖는다.

```swift
{function name}({argument value 1}, {argument value 2})
```

*함수 이름*은 그 값이 함수 타입인 표현식일 수 있다.

함수 정의가 그 매개변수들에 대한 이름을 포함한다면, 함수 호출은 반드시 인자 값 이전에 이름을 포함해야 하며, 이들은 콜론 (`:`) 으로 구분된다. 이러한 종류의 함수 호출 표현식은 다음의 형식을 갖는다.

```swift
{function name}({argument name 1}: {argument value 1}, {argument name 2}: {argument value 2})
```

함수 호출 표현식은 닫는 소괄호 바로 뒤에 클로저 표현식이 나오는 형태인 후행 클로저를 포함할 수 있다. 후행 클로저는 함수의 인자로 이해되며, 소괄호로 둘러싸인 인자들의 마지막 인자 뒤에 붙는다. 첫 번째 클로저 표현식은 레이블이 붙지 않는다; 추가적인 클로저 표현식은 인자 레이블 뒤에 붙는다. 아래의 예시는 후행 클로저 신택스를 사용하고 사용하지 않은 함수 호출의 동일한 버전을 보여준다.

```swift
// someFunction takes an integer and a closure as its arguments
someFunction(x: x, f: { $0 == 13 })
someFunction(x: x) { $0 == 13 }

// anotherFunction takes an integer and two closures as its arguments
anotherFunction(x: x, f: { $0 == 13 }, g: { print(99) })
anotherFunction(x: x) { $0 == 13 } g: { print(99) }
```

후행 클로저가 함수의 유일한 인자라면, 소괄호를 생략할 수 있다.

```swift
// someMethod takes a closure as its only argument
myData.someMethod() { $0 == 13 }
myData.someMethod { $0 == 13 }
```

클래스, 구조체, 또는 열거형 타입은 여러 메소드 중 하나를 선언하여 함수 호출 신택스에 대해 신택틱 슈거를 활성화할 수 있다.

### 이니셜라이저 표현식

*이니셜라이저 표현식*은 타입의 이니셜라이저에 대한 접근을 제공한다. 다음의 형식을 갖는다.

```swift
{expression}.init({initializer arguments})
```

타입의 새로운 인스턴스를 초기화하기 위해 함수 호출 표현식에서 이니셜라이저 표현식을 사용할 수 있다. 또한 슈퍼클래스의 이니셜라이저에 위임하기 위해 이니셜라이저 표현식을 사용할 수 있다.

```swift
class SomeSubClass: SomeSuperClass {
    override init() {
        // subclass initialization goes here
        super.init()
    }
}
```

함수처럼, 이니셜라이저는 값으로 사용될 수 있다.

```swift
// Type annotation is required because String has multiple initializers.
let initializer: (Int) -> String = String.init
let oneTwoThree = [1, 2, 3].map(initializer).reduce("", +)
print(oneTwoThree)
// Prints "123"
```

타입을 이름으로 지정한다면, 이니셜라이저 표현식을 사용하지 않고 타입의 이니셜라이저에 접근할 수 있다. 다른 모든 경우에서는, 반드시 이니셜라이저 표현식을 사용해야 한다.

```swift
let s1 = SomeType.init(data: 3)  // Valid
let s2 = SomeType(data: 1)       // Also valid

let s3 = type(of: someValue).init(data: 7)  // Valid
let s4 = type(of: someValue)(data: 5)       // Error
```

### 명시적인 멤버 표현식

*명시적인 멤버 표현식*은 이름 있는 타입, 튜플, 또는 모듈의 멤버에 접근할 수 있게 해준다. 멤버의 아이템과 식별자 사이의 구두점 (`.`) 으로 구성된다.

```swift
{expression}.{member name}
```

이름 있는 타입의 멤버는 타입의 선언이나 익스텐션의 일부분으로서 이름 지어진다.

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass()
let y = c.someProperty  // Member access
```

튜플의 멤버들은 0부터 시작하여, 그것들이 나타나는 순서에 따른 정수를 사용한 암시적인 이름을 갖는다.

```swift
var t = (10, 20, 30)
t.0 = t.1
// Now t is (20, 20, 30)
```

모듈의 멤버는 해당 모듈의 최상위 수준 선언에 접근한다.

`dynamicMemberLookup` 애트리뷰트와 함께 선언된 타입은 런타임에서 찾아지는 멤버를 포함한다.

인자들의 이름에 의해서만 달라지는 이름을 가진 메소드나 이니셜라이저를 구분하기 위해, 각각의 인자 이름 뒤에 콜론 (`:`) 이 붙는 소괄호 안에서 인자 이름을 포함시킨다. 이름을 갖지 않는 인자에 대해서는 언더스코어 (`_`) 를 작성한다. 오버로딩된 메소드들을 구분하기 위해 타입 어노테이션을 사용한다.

```swift
class SomeClass {
    func someMethod(x: Int, y: Int) {}
    func someMethod(x: Int, z: Int) {}
    func overloadedMethod(x: Int, y: Int) {}
    func overloadedMethod(x: Int, y: Bool) {}
}
let instance = SomeClass()

let a = instance.someMethod              // Ambiguous
let b = instance.someMethod(x:y:)        // Unambiguous

let d = instance.overloadedMethod        // Ambiguous
let d = instance.overloadedMethod(x:y:)  // Still ambiguous
let d: (Int, Bool) -> Void  = instance.overloadedMethod(x:y:)  // Unambiguous
```

구두점이 라인 시작에 나타난다면, 이는 암시적인 멤버 표현식이 아닌 명시적인 멤버 표현식의 일부분으로 이해된다. 예를 들어, 다음의 리스팅은 여러 줄에 걸쳐서 나뉘어진 메소드 호출 체인을 보여준다.

```swift
let x = [10, 3, 20, 15, 4]
    .sorted()
    .filter { $0 > 5 }
    .map { $0 * 100 }
```

### 후위 셀프 표현식

후위 `self` 표현식은 표현식이나 타입의 이름 바로 뒤에 `.self`가 붙는 것으로 구성된다. 다음의 형식을 갖는다.

```swift
{expression}.self
{type}.self
```

첫 번째 형식은 *expression*의 값을 평가한다. 예를 들어, `x.self`는 `x`를 평가한다.

두 번째 형식은 *type*의 값을 평가한다. 이 형식을 사용하여 타입을 값으로서 접근한다. 예를 들어, `SomeClass.self`는 `SomeClass` 타입 자체를 평가하기 때문에, 타입 수준의 인자를 받는 함수나 메소드에 넘겨줄 수 있다.

### 서브스크립트 표현식

*서브스크립트 표현식*은 상응하는 서브스크립트 선언의 접근자와 설정자를 사용한 서브스크립트 접근을 제공한다. 다음의 형식을 갖는다.

```swift
{expression}[{index expressions}]
```

서브스크립트 표현식의 값을 평가하기 위해, *expression*의 타입에 대한 서브스크립트 접근자는 서브스크립트 매개변수로서 *인덱스 표현식*과 함께 호출된다. 값을 설정하려면 같은 방식으로 서브스크립트 설정자를 호출하면 된다.

### 강제-값 표현식

*강제-값 표현식*은 `nil`이 아니라고 확신하는 옵셔널 값을 언래핑한다. 다음의 형식을 갖는다.

```swift
{expression}!
```

*expression*의 값이 `nil`이 아니면, 옵셔널 값은 언래핑되고 상응하는 옵셔널이 아닌 타입으로 반환된다. 그렇지 않으면, 런타임 에러가 발생한다.

강제-값 표현식의 언래핑된 값은 수정될 수 있으며, 값 그 자체를 변경하거나, 값의 멤버 중 하나를 할당하여 수정될 수 있다.

```swift
var x: Int? = 0
x! += 1
// x is now 1

var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
someDictionary["a"]![0] = 100
// someDictionary is now ["a": [100, 2, 3], "b": [10, 20]]
```

### 옵셔널 체이닝 표현식

*옵셔널 체이닝 표현식*은 후위 표현식에서 옵셔널 값을 사용하기 위한 단순화된 신택스를 제공한다. 다음의 형식을 갖는다.

```swift
{expression}?
```

후위 연산자 `?`는 표현식의 값을 변경하지 않고 표현식으로부터 옵셔널-체이닝 표현식을 만든다.

옵셔널-체이닝 표현식은 반드시 후위 표현식에서 나타나야 하며, 이는 후위 표현식이 특별한 방식으로 평가되게 한다. 옵셔널-체이닝 표현식의 값이 `nil`이면, 후위 표현식에 있는 모든 다른 오퍼레이션은 무시되고 전체 후위 표현식은 `nil`로 평가된다. 옵셔널 체이닝 표현식의 값이 `nil`이 아니라면, 옵셔널-체이닝 표현식의 값은 언래핑되고 나머지 후위 표현식을 평가하기 위해 사용된다. 각각의 경우에서 후위 표현식의 값은 여전히 옵셔널 타입이다.

옵셔널-체이닝 표현식을 포함하는 후위 표현식은 다른 후위 표현식 안에 중첩되며, 오직 가장 바깥쪽에 있는 표현식만이 옵셔널 타입을 반환한다. 아래의 예시에서, `c`가 `nil`이 아닐 때, 그 값은 언래핑되고 `.property`를 평가하기 위해 사용되며, 이 값은 `.performAction()`을 평가하기 위해 사용된다. 전체 표현식 `c?.property.performAction()`은 옵셔널 타입의 값을 갖는다.

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction()
```

다음의 예시는 옵셔널 체이닝을 사용하지 않은 위의 예시의 동작을 보여준다.

```swift
var result: Bool?
if let unwrappedC = c {
    result = unwrappedC.property.performAction()
}
```

옵셔널-체이닝 표현식의 언래핑된 값은 수정될 수 있으며, 값 그 자체를 변경하거나, 그 값의 멤버 중 하나로 할당하여 수정할 수 있다. 옵셔널-체이닝 표현식의 값이 `nil`이면, 할당 연산자 오른쪽에 있는 표현식은 평가되지 않는다.

```swift
func someFunctionWithSideEffects() -> Int {
    return 42  // No actual side effects.
}
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["not here"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects is not evaluated
// someDictionary is still ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["a"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects is evaluated and returns 42
// someDictionary is now ["a": [42, 2, 3], "b": [10, 20]]
```