# A Tour of the Dart Language

## 중요 개념

- 변수에 할당할 수 있는 모든 것은 *오브젝트*이며, 모든 오브젝트는 *클래스*의 인스턴스이다. 숫자, 함수, `null`도 오브젝트이다. 모든 오브젝트는 `Object` 클래스를 상속받는다.
- 강타입 언어이지만 타입 추론을 지원하므로 타입을 반드시 명시할 필요는 없다. 명시적으로 어떠한 타입도 기대하지 않는다는 것을 원한다면 `dynamic`이라는 특별한 타입을 사용하라.
- `List<int>`나 `List<dynamic>` 같은 제네릭 타입을 지원한다.
- `main()`과 같은 전역 함수를 지원하며, 클래스나 오브젝트에 묶인 함수 *(정적 메소드 및 인스턴스 메소드*) 또한 지원한다. 함수 내에 함수를 생성할 수 있다. (*중첩* 또는 *지역* 함수)
- 전역 변수를 지원하며, 클래스나 오브젝트에 묶인 변수 (*정적 변수 또는 인스턴스 변수*) 또한 지원한다. 인스턴스 변수는 *필드* 또는 *프로퍼티*로도 불린다.
- `public`, `protectd`, `private`와 같은 접근 지정자 키워드를 지원하지 않는다. 식별자가 밑줄(`_`)로 시작한다면 그 라이브러리에서 private 공개 수준을 갖게 된다.
- *식별자*는 문자나 밑줄(`_`)로 시작할 수 있으며, 이후에 어떠한 조합의 문자나 숫자가 올 수 있다.
- *표현식*(런타임 값을 취함)과 *선언문*(런타임 값이 아님)을 지원한다. 선언문은 하나 이상의 표현식을 포함하나, 표현식은 직접적으로 선언문을 포함할 수 없다.
- 경고는 작동하지 않을 코드를 알려주는 것이며, 프로그램 실행을 막지는 않는다. 에러는 컴파일 타임이나 런타임에 일어날 수 있다. 컴파일 타임 에러는 코드 실행을 막는다. 런타임 에러는 코드 실행 중 예외를 발생시킨다. 

## 키워드

- 키워드를 식별자로 사용하는 것을 지양하자.
- `show` / `async` / `sync` / `on` / `hide`
  - 문맥상 키워드. 특정 위치에서만 의미를 가짐. 어디에서든 유효한 식별자로 사용될 수 있다.
- `abstract` / `dynamic` / `implements` / `import` / `as` / `export` / `interface` / `external` / `library` / `factory` / `mixin` / `typedef` / `operator` / `covariant` / `Function` / `part` / `get` / `deferred` / `set`
  - 내장 식별자. 대부분의 자리에서 유효한 식별자로 사용될 수 있으나, 클래스나 타입 이름으로 사용될 수 없고, import 접두사로도 사용될 수 없다.
- `await` / `yield`
  - 비동기를 지원하기 위한 예약어. 이 키워드들은 `async`, `async*`, `sync*`가 명시된 함수의 구현부에서 사용될 수 없다.
- `else` / `assert` / `enum` / `in` / `super` / `switch` / `extends` / `is` / `break` / `this` / `case` / `throw` / `catch` / `false` / `new` / `true` / `class` / `final` / `null` / `try` / `const` / `finally` / `continue` / `for` / `var` / `void` / `default` / `rethrow` / `while` / `return` / `with` / `do` / `if`
  - 나머지 키워드들은 예약어로서, 식별자로 사용될 수 없다.

## 변수

```dart
// `name`은 String 타입으로 추론됨
var name = "Bob";
// 아래와 같이 타입을 명시해줄 수도 있음
String name = "Bob";
```

- 문자열에는 큰따옴표나 작은따옴표를 사용하여 감싼다.
- 세미콜론을 문장 끝에 붙여야 한다.
- `dynamic` 또는 `Object` 키워드로 변수를 선언하여 해당 변수에 들어갈 수 있는 타입이 다르게 해줄 수 있다.
  - Dart는 강타입 언어.

### 기본값

- 초기화되지 않은 변수는 `null`을 초기값으로 갖는다. Dart에서 모든 타입은 오브젝트이므로 숫자 타입의 변수도 초기값을 `null`로 갖는다.

### final 및 const

```dart
// `name`은 변경될 수 없으며, String 타입으로 추론됨
final name = "Bob";
// 아래와 같이 타입을 명시한 상수를 선언할 수 있음
final String name = "Bob";
```

- 변수를 변화시킬 의도가 전혀 없다면, `var` 대신  `final`이나 `const`를 사용하라. 
  - `final` 변수는 오직 한 번만 설정 가능하다.
  - `const` 변수는 컴파일 타임 상수이다. `const` 변수는 암시적으로 `final`이다.
- `final` 전역변수 또는 클래스 변수는 사용되는 처음에 초기화된다.
- 인스턴스 변수는 `const`가 아닌 `final`이다. `final` 인스턴스 변수는 생성자 구현부 시작 전에 초기화되어야 한다. 
  - 변수 선언부, 생성자*constructor* 매개변수, 생성자 이니셜라이저 리스트
- `const`는 컴파일 타임 상수를 원할 때 사용하라.
- `const`는 상수*값*을 생성할 때도 사용할 수 있음

```dart
// array는 이제 다른 것을 가리킬 수 없다. (array에 다른 리스트를 할당할 수 없다.)
// array가 가리키는 빈 리스트에 값을 추가할 수 있다.
final array = [];
// array는 이제 다른 것을 가리킬 수 없다.
// array가 가리키는 빈 리스트는 const이므로 값을 추가할 수 없다.
final array = const [];
// const로 선언된 변수는 할당된 참조도 const이다.
// const array = const []; 와 같은 코드이나 const를 중복되게 사용하지 말자.
const array = [];
```

> Dart에서 모든 타입은 클래스이며, 참조 타입이다.

## 내장 타입

- 숫자 / 문자열 / 불리언 / 리스트(*배열*) / 맵*maps* / 런*runes*(유니코드 문자로 나타내어진 문자열) / 심볼*symbols*
- 리터럴 문법을 사용하여 위의 오브젝트를 초기화할 수 있음
- 일반적으로 변수를 초기화하기 위해 생성자*constructor*를 사용함

### 숫자

- 두 종류가 있다.
  - **int** : 플랫폼에 따라, 64비트를 넘지 않는 정수 값
  - **double** : 64비트 부동 소수점
    - Dart 2.1 이전에는 double 문맥에 정수 리터럴을 사용하면 에러가 났다.
- `int`와 `double`은 `num`의 하위 타입.
  - `dart:math` 라이브러리에서 `num` 타입이 지원하지 않는 것들을 찾아볼 수 있다.

```dart
// int형 변수 x
var x = 1;
// double형 변수 y
var y = 1.2;
// 문자열을 정수로 파싱
var one = int.parse("1");
// 문자열을 실수로 파싱
var onePointOne = double.parse("1.1");
// 정수를 문자열로 파싱
var oneAsString = 1.toString();
// 실수를 문자열로 파싱. 소수점 아래 둘째 자리까지 유지
var piAsString = 3.14159.toStringAsFixed(2);
```

### 문자열

- Dart에서 문자열은 UTF-16 코드 단위의 시퀀스. 작은따옴표 또는 큰따옴표를 사용하여 문자열을 생성할 수 있다.
- 문자열 내에 `${expression}`을 사용하여 표현식의 값을 넣을 수 있다. 표현식이 식별자라면 `{}`를 생략할 수 있다.
- 오브젝트와 상응하는 문자열을 얻기 위해 오브젝트의 `toString()` 메소드를 호출하기.
- `+` 연산자로 문자열을 이어붙일 수 있음
- 멀티 라인 문자열을 지원. Swift와 같은 방법으로 작성. (세 개의 작은따옴표 또는 큰따옴표 사이에 작성)
- `r` 접두사를 붙여 *raw string*을 생성할 수 있음
  - 이스케이핑 문자, 문자열 보간법 등이 적용되지 않음

```dart
// In a raw string, not even \n gets special treatment.
var s = r"In a raw string, not even \n gets special treatment.";
```

- 유니코드 문자로 표현된 문자열을 만들고 싶다면 runes를 참고하기
- `const` 문자열에 문자열 보간법을 사용하여 `const` 변수를 포함할 수 있으나, 일반 변수는 포함할 수 없음

### 불리언

- `bool` 타입 사용. `true` 및 `false`. 컴파일 타임 상수.

### 리스트

- 일반적으로 *array*와 의미가 같음
- 대괄호 리터럴을 사용하여 리스트 초기화 가능

### 맵

- 키와 값의 쌍. Swift의 딕셔너리.
  - 키는 유일해야 함
- 중괄호 사이에 키와 값을 콜론으로 구분하여 초기화 가능, `Map` 타입

```dart
// Map<String, String> 타입으로 추론됨
var gifts = {
    "first": "partridge",
    "second": "turtledoves",
    "fifth": "golden rings"
};
// Map 생성자를 사용할 수도 있음. Swift와 비슷
var gifts = Map();
gifts[1] = "asdf";
gifts[3] = "fsad";
```

### 룬*Runes*

- UTF-32 (유니코드)로 구성된 문자열
- 문자열 (`String`)과 이것을 구분하여 사용해야 함
  - 문자열은 UTF-16

### 심볼*Symbols*

- `#` 다음 식별자 이름을 위치시킴. `#radix`
- 심볼 오브젝트는 Dart 프로그램에서 선언된 연산자나 식별자를 나타냄
- 이름에 의해 식별자가 참조되는 API에서 매우 중요함
- 컴파일 타임 상수.

## 함수

- 함수 또한 오브젝트이고 `Function` 타입을 갖는다.
  - 함수는 변수에 할당될 수 있고 다른 함수에 인자로 넘겨질 수 있다.
  - Dart 클래스의 인스턴스를 마치 함수인 것처럼 호출할 수 있다.

```dart
bool isEven(int number) {
    return number % 2 == 0;
}
// 위아래 코드는 같은 역할을 함. 한줄 함수나 메소드는 아래와 같이 정의 가능
// 아래와 같이 arrow syntax를 사용할 때 선언문을 사용할 수 없음. 오직 표현식만 위치할 수 있다.
bool isEven(int number) => number % 2 == 0;
```

- 반환형을 생략할 수 있으나 그렇게 하지 않도록 하자.
- 두 가지 타입의 매개변수를 갖는다.
  - required / optional
  - required 매개변수는 어떠한 optional 매개변수보다도 앞에 위치한다.
  - 이름 있는 optional 매개변수는 `@required`로 마크될 수 있다.

### 옵셔널 매개변수

- positional 이거나 named일 수 있으나, 둘 다일 수는 없다.
  - 매개변수 리스트에 `[]`와 `{}`가 함께 존재할 수 없다.

**옵셔널 이름 있는 매개변수*Optional named parameters***

- 함수 정의시 매개변수들을 중괄호 안에 감싸서 Swift의 전달인자 레이블의 효과를 낼 수 있다.

```dart
bool isEven(int number) => number % 2 == 0;
isEven(3);
////
bool isEven({int number}) => number % 2 == 0;
isEven(number: 3);
```

위의 코드를 Swift로 작성하자면...

```swift
func isEven(_ number: Int) -> Bool { return number % 2 == 0 }
isEven(3)
////
func isEven(number: Int) -> Bool { return number % 2 == 0 }
isEven(number: 3)
```

- 이름 있는 매개변수에 `@required`를 명시하여 필수 매개변수로 지정할 수 있다.
  - `package:meta/meta.dart`를 임포트해야 사용 가능
  - `package:flutter/material.dart`가 위의 패키지를 임포트하고 있음

**옵셔널 위치 매개변수*Optional positional parameters***

- 대괄호로 매개변수를 감싸기. 해당 매개변수에 값이 전달될 수도 있고 전달되지 않을 수도 있다.
  - 전달되지 않을 경우 해당 인자는 `null`이다.

**기본 매개변수 값**

- 옵셔널 매개변수 나열시 매개변수 기본값을 설정해줄 수 있음. 기본값은 컴파일 타임 상수여야 한다.
- 필수 매개변수에는 매개변수 기본값을 설정할 수 없다.

```dart
bool isEven({int number = 3}) => number % 2 == 0;
// false
isEven()
```

- 이름 있는 매개변수는 이름으로 참조되므로 그 순서에 신경쓰지 않아도 된다.

> optional positional parameters과 optional named parameters의 차이는 Swift의 전달인자 레이블 효과를 낼 수 있는지에 대한 것이다.?

### main() 함수

- 모든 애플리케이션은 `main()` 전역함수를 가지고 있음
  - 애플리케이션의 진입점
  - optional `List<String>` 매개변수를 인자로 받음.

### 일급 객체로서의 함수

- 함수를 다른 함수의 매개변수로 넘길 수 있음
- 함수를 변수에 할당할 수 있음

```dart
// dynamic 타입 매개변수를 받아 String 타입을 반환하는 변수
var loudify = (message) => "${message.toUpperCase()}";
```

### 익명 함수

- 익명 함수 / 람다 / 클로저
- Swift의 클로저와 다른 점
  - 매개변수는 소괄호로 둘러싸여야 함
  - 중괄호가 클로저 구현부 전체를 둘러싸는 것이 아니라 매개변수 다음에 위치함
    - `in` 뒤에 클로저 구현부가 위치하는 것이 아니라 중괄호 안에 클로저를 구현함
  - 클로저 구현부가 한 줄일 경우 arrow notation 사용 가능
  - 후행 클로저 / 클로저 축약 문법 없음

```dart
var list = [1, 2, 3];
list.forEach((item) {
    print(item);
})
```

```swift
var list = [1, 2, 3];
list.forEach({ item in 
	print(item)
})
```

### Lexical scope

- 코드의 레이아웃에 의해 정적으로 변수 스코프가 결정됨
- 중첩 함수 내에서 스코프 바깥에 있는 변수에 접근할 수 있음

### Lexical closures

- 클로저의 값 캡쳐에 대한 내용. Swift와 다를 것 없음

### 함수 동등 테스트

- `==` 연산자는 인스턴스의 주소를 비교함

### 반환 값

- 모든 함수는 값을 반환하며, 반환 값이 명시되지 않았다면 `null`을 반환한다.
  - `return null;`이 암시적으로 함수 구현부에 위치한 것

## 연산자

- 많은 연산자가 정의되어 있으며, 재정의 가능한 연산자에 한하여 재정의도 가능하다.
- 연산자를 사용하여 표현식을 작성할 수 있다.
- 우선순위는 생각하지 말고 소괄호를 사용하여 가독성 좋게 작성하자.

### 산술 연산자

- 다른 언어와 다를 것 없음
  - `~/` : 정수 값을 반환하는 나누기
  - `/` : 나누기
- Swift와는 다르게 C언어 스타일의 단항 연산자 제공
  - `++` 및 `--`

### 비교 및 관계 연산자

- 다른 언어와 다를 것 없음
  - `==` 연산자를 사용하여 두 오브젝트가 같은 것을 나타내는지 확인할 수 있음
  - 두 오브젝트가 완벽하게 동일한 것인지 비교하고 싶으면 `==` 연산자 대신 `identical()` 함수를 사용하라.

### 타입 테스트 연산자

- Swift와 비슷
  - `as` : 타입 캐스팅
    - 타입 캐스팅이 실패하면 예외를 던짐
  - `is` : 오브젝트가 특정 타입이라면 `true`
  - `is!` : 오브젝트가 특정 타입이라면 `false`

### 할당 연산자

- 다른 언어와 다를 것 없음
  - `~/=` : 정수 값을 반환하는 나누기 결과를 할당

### 논리 연산자

- 다른 언어와 다를 것 없음

### 비트와이즈 및 쉬프트 연산자

- 다른 언어와 다를 것 없음

### 논리 표현식

- 삼항 연산자 지원
- Swift처럼 `a ?? b`를 지원한다! `a`가 `null`이면 `b`를 반환하고, 그렇지 않으면 `a`를 반환한다.
  - Swift는 옵셔널 바인딩에 `??` 연산자가 사용됨
  - Dart는 `null` 체크용

### 캐스케이드 표기법*Cascade notation*

- 캐스케이드 (`..`)는 같은 오브젝트에서 연산 시퀀스를 만들게 해준다. 함수 호출에 더하여 같은 오브젝트에서 필드에 접근할 수 있다.
- 명백하게 말하여 캐스케이드를 위한 *double dot* 표기법은 연산자가 아니다.

```dart
// 마지막 캐스케이드 표기법에 세미콜론 붙이기.
void main() {
  var c = A()
  ..a = 3
  ..b = 4;
}
class A {
  int a;
  int b;
}
```

- 실제 오브젝트를 반환하는 함수에 캐스케이드 표기법을 사용할 때 조심하기. `void`를 반환하는 호출 후 캐스케이드 표기법을 사용할 수 없다.

### 다른 연산자

- 다른 언어와 다를 것 없음
- `?.` : 조건 멤버 접근 연산자. 접근하려는 왼쪽 값의 `null` 여부를 확인하여 `null`이 아니라면 오른쪽 값에 접근
  - Swift에서는 옵셔널 체이닝. Dart에서는 `null` 체크

## 조건문

### if 및 else

- 다른 언어와 다를 것 없음
- 조건에는 `bool` 타입만 들어갈 수 있으며, 소괄호로 감싸져야 한다.

### for 반복문

- C언어 스타일의 for문 사용하기
- 반복 가능한 오브젝트에 대하여 `forEach()` 메소드를 사용하여 순환할 수 있음
- `for-in` 구문도 사용 가능

```dart
var array = [0, 1, 2];
for (var x in array) {
    print(x);
}
```

### while 및 do-while

- 다른 언어와 다를 것 없음

### break 및 continue

- 다른 언어와 다를 것 없음

### switch 및 case

- 조건으로 정수, 문자열 또는 컴파일 타임 상수가 들어갈 수 있다.
- 열거형 타입이 `switch`문과 궁합이 잘 맞는다.
- 비지 않은 `case`절은 `break`, `continue`, `throw`, `return` 등의 키워드를 사용하여 `case`절을 끝내야 함
- 빈 `case` 절에는 `break` 등을 명시할 필요가 없으며 자동으로 fallthrough 된다.
- 각 `case`절은 지역 변수를 가질 수 있다.

### Assert

- `assert`문을 사용하여 조건이 `false`일 때 실행을 막을 수 있다. (AssertionError 예외를 던짐)
- 배포용 빌드에서는 효과가 없다.
- Flutter는 디버그 모드에서 assert를 활성화한다.
- 두 번째 인자에 문자열을 추가하여 assert에 메세지를 붙일 수 있다.

## 예외