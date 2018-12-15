# A Tour of the Dart Language

이 페이지는 변수와 연산자에서 클래스와 라이브러리에 이르기까지, Dart의 주요한 특징 각각을 사용하는 방법을 보여줍니다. 다른 언어로 프로그래밍하는 방법을 이미 알고 있다는 가정 하에서 말입니다.

Dart의 코어 라이브러리에 대해 더 배우고 싶다면 [Dart 라이브러리 투어](https://www.dartlang.org/guides/libraries/library-tour)를 참고하십시오. 언어 특징에 대한 세부 사항을 알고 싶을 때마다 [Dart 언어 명세](https://www.dartlang.org/guides/language/spec)를 참고하십시오.

> **팁.** DartPad를 사용하여 Dart의 언어 특징 대부분을 가지고 놀 수 있습니다. [더 배우기](https://www.dartlang.org/tools/dartpad) 
> 
> [**DartPad 열기**](https://dartpad.dartlang.org/)

## Dart 기본 프로그램

다음의 코드는 Dart의 가장 기본적인 특징을 많이 사용합니다.

```dart
// 함수 정의
printInteger(int aNumber) {
  print('The number is $aNumber.'); // 콘솔에 출력
}

// 앱이 실행을 시작하는 곳
main() {
  var number = 42; // 변수 선언 및 초기화
  printInteger(number); // 함수 호출
}
```

이 프로그램이 모든 (거의 대부분의) Dart 앱에 적용되는 것을 사용한 것이 여기 나와 있습니다.

`// 주석`

한줄 주석. Dart는 여러줄 주석과 문서화 주석도 지원합니다. [주석](https://www.dartlang.org/guides/language/language-tour#comments)을 참고하십시오.

`int`

타입. 다른 [내장 타입](https://www.dartlang.org/guides/language/language-tour#built-in-types) 중 몇몇에는 `String`, `List`, `bool`이 있습니다.

`42`

숫자 리터럴. 숫자 리터럴은 컴파일 타임 상수의 종류 중 하나입니다.

`print()`

출력을 표시하는 손쉬운 방법.

`'...'` (`"..."`)

문자열 리터럴.

`$변수명` (`${표현식}`)

문자열 보간법. 문자열 리터럴 안에 변수나 표현식의 문자열 표현을 포함하는 것. [문자열](https://www.dartlang.org/guides/language/language-tour#strings)을 참고하십시오.

`main()`

앱 실행이 시작하는, 특별하고, *필수적인*, 전역 함수. [main() 함수](https://www.dartlang.org/guides/language/language-tour#the-main-function)를 참고하십시오.

`var`

타입을 지정하지 않고 변수를 선언하는 방법.

> **알아두기** 이 사이트의 코드는 [Dart 스타일 가이드](https://www.dartlang.org/guides/language/effective-dart/style)에 있는 관습을 따릅니다.

## 중요 개념

- 변수에 위치할 수 있는 모든 것은 *오브젝트*이며, 모든 오브젝트는 *클래스*의 인스턴스입니다. 심지어 숫자, 함수, 그리고 `null`도 오브젝트입니다. 모든 오브젝트는 [Object](https://api.dartlang.org/stable/dart-core/Object-class.html) 클래스로부터 상속받습니다.
- Dart가 강타입 언어일지라도 타입을 추론할 수 있기 때문에 타입을 명시하는 것은 선택적입니다. 위의 코드에서 `number`는 `int` 타입이 되도록 추론됩니다. 어떠한 타입도 기대하지 않는다고 명시적으로 말하기 원할 때, [특별한 타입 dynamic을 사용](https://www.dartlang.org/guides/language/effective-dart/design#do-annotate-with-object-instead-of-dynamic-to-indicate-any-object-is-allowed)하십시오.
- Dart는 `List<int>`(정수 리스트)나 `List<dynamic>`(어떠한 타입을 갖는 오브젝트 리스트)와 같은 제네릭 타입을 지원합니다.
- Dart는 `main()`과 같은 전역 함수뿐만 아니라, 클래스나 오브젝트에 묶인 함수(각각 *정적* 메소드, *인스턴스* 메소드)도 지원합니다. 또한 함수 내 함수(*중첩* 또는 *지역* 함수)를 만들 수 있습니다.
- 이와 비슷하게 Dart는 전역 변수뿐만 아니라, 클래스나 오브젝트에 묶인 변수(각각 *정적* 변수, *인스턴스* 변수)도 지원합니다. 인스턴스 변수는 때때로 필드 혹은 프로퍼티라고도 알려져 있습니다.
- Java와는 다르게, Dart는 `public`, `protected`, `private` 키워드를 가지고 있지 않습니다. 식별자가 밑줄(_)로 시작한다면, 그것은 그 라이브러리에서 private합니다. [라이브러리 및 가시성](https://www.dartlang.org/guides/language/language-tour#libraries-and-visibility)을 참고하십시오.
- *식별자*는 문자나 밑줄(_)로 시작할 수 있으며, 이후에는 문자와 숫자를 더한 어떠한 조합도 올 수 있습니다.
- Dart는 *표현식*(런타임 값을 가짐)과 *선언문*(런타임 값을 갖지 않음) 모두를 가지고 있습니다. 예를 들어 [조건 표현식](https://www.dartlang.org/guides/language/language-tour#conditional-expressions) `condition ? expr1 : expr2`는 `expr1` 또는 `expr2`의 값을 갖습니다. 값을 갖지 않는 [if-else문]()과 비교해 보십시오. 선언문은 종종 하나 또는 그 이상의 표현식을 포함하나 표현식은 선언문을 직접적으로 포함할 수 없습니다.
- Dart 툴은 두 종류의 문제 *경고*와 *에러*를 보고할 수 있습니다. 경고는 단지 코드가 동작하지 않을 수 있다는 것을 가리키지만 프로그램 실행을 막지는 않습니다. 에러는 컴파일 타임 또는 런타임에 발생할 수 있습니다. 컴파일 타임 에러는 코드 실행을 막습니다. 런타임 에러는 코드가 실행되는 동안 발생한 [예외](https://www.dartlang.org/guides/language/language-tour#exceptions)의 결과를 불러옵니다.

## 키워드

다음의 테이블은 Dart 언어가 특별하게 취급하는 단어들을 나열합니다.

이러한 단어를 식별자로 사용하지 마십시오. 그러나 필요하다면 다음의 키워드는 식별자가 될 수 있습니다.

- `show` / `async` / `sync` / `on` / `hide`
  - 문맥상 키워드가 될 수 있으며, 이는 오직 특정 장소에서만 의미를 갖는다는 것을 의미합니다. 이들은 어느 곳에서나 식별자로 사용될 수 있습니다.
- `abstract` / `dynamic` / `implements` / `import` / `as` / `export` / `interface` / `external` / `library` / `factory` / `mixin` / `typedef` / `operator` / `covariant` / `Function` / `part` / `get` / `deferred` / `set`
  - 내장 식별자. JavaScipt 코드를 Dart로 포팅하는 작업을 단순화하기 위해 이러한 키워드들은 대부분의 장소에서 식별자로 사용될 수 있습니다. 하지만 클래스나 타입 이름, 또는 import 접두사로 사용될 수 없습니다.
- `await` / `yield`
  - Dart 1.0 릴리즈 이후 추가된, [비동기 지원](https://www.dartlang.org/guides/language/language-tour#asynchrony-support)과 관련된 새롭고 제한된 예약어입니다. `async`, `async*`, `sync*`로 마크된 어떠한 함수의 구현부에서 `await`나 `yield`를 식별자로 사용할 수 없습니다.
- `else` / `assert` / `enum` / `in` / `super` / `switch` / `extends` / `is` / `break` / `this` / `case` / `throw` / `catch` / `false` / `new` / `true` / `class` / `final` / `null` / `try` / `const` / `finally` / `continue` / `for` / `var` / `void` / `default` / `rethrow` / `while` / `return` / `with` / `do` / `if`
  - 나머지 키워드들은 **예약어**로서, 식별자로 사용될 수 없습니다.

## 변수

변수를 만들고 초기화하는 예제입니다.

```dart
var name = 'Bob';
```

변수는 참조를 저장합니다. `name`이라고 불리는 변수는 "Bob"이라는 값을 가지고 있는 `String` 오브젝트에 대한 참조를 포함합니다.

`name` 변수의 타입은 `String`으로 추론됩니다. 하지만 이를 지정하여 타입을 바꿀 수 있습니다. 오브젝트가 하나의 타입으로 제한되지 않는다면, [디자인 가이드라인](https://www.dartlang.org/guides/language/effective-dart/design#do-annotate-with-object-instead-of-dynamic-to-indicate-any-object-is-allowed)에 따라 `Object`나 `dynamic` 타입을 지정하십시오.

```dart
dynamic name = 'Bob';
```

다른 선택지는 추론될 타입을 명시적으로 선언하는 것입니다.

```dart
String name = 'Bob';
```

> **알아두기** 이 페이지는 지역 변수에 대해 타입 지정보다는, [스타일 가이드 추천](https://www.dartlang.org/guides/language/effective-dart/design#do-annotate-with-object-instead-of-dynamic-to-indicate-any-object-is-allowed)에 따라 `var`를 사용합니다.

### 기본값

초기화되지 않은 변수는 초기값으로 `null`을 갖습니다. 숫자 타입의 변수도 초기에는 null인데, 숫자도 Dart의 다른 모든 것들처럼 오브젝트이기 때문입니다.

```dart
int lineCount;
assert(lineCount == null);
```

### final 및 const

변수를 변경할 의도가 전혀 없다면, `var`나 타입에 더하기보다는, `final`이나 `const`를 사용하십시오. final 변수는 오직 한 번만 설정될 수 있습니다. const 변수는 컴파일 타임 상수입니다. (const 변수는 암시적으로 final입니다.) final 전역 변수 또는 클래스 변수는 그것이 사용되는 최초 시점에 초기화됩니다.

> **알아두기** 인스턴스 변수는 `final`은 될 수 있으나 `const`는 될 수 없습니다. final 인스턴스 변수는 생성자 구현부가 시작하기 전에 초기화되어 있어야 합니다. 변수 선언, 생성자 매개변수, 또는 생성자의 [이니셜라이저 리스트](https://www.dartlang.org/guides/language/language-tour#initializer-list)에서 그렇게 되어야 합니다.

final 변수를 만들고 설정하는 예제가 있습니다.

```dart
final name = 'Bob'; // 타입 지정 없이 선언
final String nickname = 'Bobby';
```

final 변수의 값을 변경할 수 없습니다.

```dart
// error
name = 'Alice'; // Error: final 변수는 오직 한 번만 설정될 수 있습니다.
```

**컴파일 타임 상수**를 원한다면 `const`를 사용하십시오. const 변수가 클래스 레벨이 있다면, `static const`로 마크하십시오. 변수를 선언하는 곳에서 숫자나 문자열 리터럴, const 변수, 또는 상수에 대한 산술 연산의 결과와 같은 값을 컴파일 타임 상수에 설정하십시오.

```dart
const bar = 1000000;  // 감압 단위 (dynes/cm2)
const double atm = 1.01325 * bar; // 표준 기압
```

`const` 키워드는 상수 선언에만 사용되지 않습니다. 상수 *값*을 만들 때도 사용 가능하며, 상수 값을 *만드는* 생성자를 선언할 때도 사용 가능합니다. 어떠한 변수라도 상수 값을 가질 수 있습니다.

```dart
var foo = const [];
final bar = const [];
const baz = []; // `const []`와 같음
```

위에 있는 `baz`처럼, `const` 선언의 초기화 표현식에서 `const`를 생략할 수 있습니다. [const를 중복으로 사용하지 마십시오](https://www.dartlang.org/guides/language/effective-dart/usage#dont-use-const-redundantly)를 참고하십시오.

final이 아닌 변수, const가 아닌 변수, 심지어 그것이 상수 값을 갖기 위해 사용될지라도 그 값을 변경할 수 있습니다.

```dart
foo = [1, 2, 3];  // 이전에는 const [] 였음
```

const 변수의 값을 변경할 수 없습니다.

```dart
// Error
baz = [42]; // Error: 상수 선언된 변수에 값을 할당할 수 없습니다.
```

상수 값을 만들기 위해 `const`를 사용하는 더 많은 정보는 [리스트](https://www.dartlang.org/guides/language/language-tour#lists), [맵](https://www.dartlang.org/guides/language/language-tour#maps), [클래스](https://www.dartlang.org/guides/language/language-tour#classes)를 참고하십시오.

## 내장 타입

Dart 언어는 다음의 타입에 대한 특별한 지원을 제공합니다.

- 숫자
- 문자열
- 불리언
- 리스트 (*배열*이라고도 알려져 있음)
- 맵
- 룬 (문자열에서 유니코드 문자를 표현하기 위함)
- 심볼

이러한 특별한 타입들은 리터럴을 사용하여 오브젝트를 초기화할 수 있습니다. 예를 들어 `'this is a string'`은 문자열 리터럴이고, `true`는 불리언 리터럴입니다.

Dart의 모든 변수는 오브젝트(*클래스*의 인스턴스)를 참조하기 때문에 변수를 초기화하기 위해 *생성자*를 보통 사용할 수 있습니다. 몇몇 내장 타입은 그만의 생성자를 가지고 있습니다. 예를 들어 맵을 만들기 위해 `Map()` 생성자를 사용할 수 있습니다.

### 숫자

Dart의 숫자는 두 가지 종류로 나뉩니다.

- [**int**](https://api.dartlang.org/stable/dart-core/int-class.html)
  - 정수 값은 그 플랫폼에 따라 64비트보다 길지 않습니다. Dart VM에서 값은 -2<sup>63</sup>에서 2<sup>63</sup> - 1까지 될 수 있습니다. JavaScript에서 컴파일된 Dart는 [JavaScript 숫자](https://stackoverflow.com/questions/2802957/number-of-bits-in-javascript-numbers/2803010#2803010)를 사용하여 -2<sup>53</sup>에서 2<sup>53</sup> - 1까지 될 수 있습니다.
- [**double**](https://api.dartlang.org/stable/dart-core/double-class.html)
  - IEEE 754 표준에 의해 지정된 64비트(두 배 정밀도) 부동 소수점 숫자. 

`int`와 `double`은 `num`의 하위 타입입니다. num 타입은 +, -, /, *와 같은 기본 연산자를 포함합니다. 또한 `abs()`, `ceil()`, `floor()`와 같은 메소드도 가지고 있습니다. (>>와 같은 비트와이즈 연산자는 `int` 클래스에 정의되어 있습니다.) num과 그 하위 타입이 당신이 찾고 있는 것을 가지고 있지 않다면, [dart:math](https://api.dartlang.org/stable/dart-math) 라이브러리는 가지고 있을지도 모릅니다.

정수는 소수점이 없는 숫자입니다. 정수 리터럴을 정의하는 예제가 여기 있습니다.

```dart
var x = 1;
var hex = 0xDEADBEEF;
```

숫자가 소수를 포함한다면, 그것은 double입니다. double 리터럴을 정의하는 예제가 여기 있습니다.

```dart
var y = 1.1;
var exponents = 1.42e5;
```

Dart 2.1에서 정수 리터럴은 필요하다면 double로 자동으로 변환됩니다.

```dart
double z = 1; // double z = 1.0과 같음
```

> **버전 기록** : Dart 2.1 이전에서 double 컨텍스트에 정수 리터럴을 사용하는 것은 에러를 발생시켰습니다.

문자열을 숫자로, 또는 그 반대로 변환하는 방법이 여기 있습니다.

```dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```

int 타입은 전통적인 비트와이즈 쉬프트 (<<, >>), AND (&), OR(|) 연산자를 지정합니다.

```dart
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 >> 1) == 1); // 0011 >> 1 == 0001
assert((3 | 4) == 7); // 0011 | 0100 == 0111
```

리터럴 숫자는 컴파일 타임 상수입니다. 많은 산술 표현식 또한, 피연산자들이 숫자 값을 구하는 컴파일 타임 상수인 한, 컴파일 타임 상수입니다.

```dart
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```

### 문자열

Dart의 문자열은 UTF-16 코드 단위의 시퀀스입니다. 문자열을 만들기 위해 작은따옴표 또는 큰따옴표를 사용할 수 있습니다.

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

`${표현식}`을 사용하여 문자열 안에 표현식의 값을 넣을 수 있습니다. 표현식이 식별자라면 {}을 생략할 수 있습니다. 오브젝트에 대응하는 문자열을 얻기 위해 Dart는 오브젝트의 `toString()` 메소드를 호출합니다.

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
    'Dart has string interpolation, ' +
        'which is very handy.');
assert('That deserves all caps. ' +
        '${s.toUpperCase()} is very handy!' ==
    'That deserves all caps. ' +
        'STRING INTERPOLATION is very handy!');
```

> **알아두기** : `==` 연산자는 두 오브젝트가 동일한지 검증합니다. 두 개의 문자열은 같은 코드 단위의 시퀀스를 포함한다면 동일합니다.

인접한 문자열 리터럴 또는 `+` 연산자를 사용하여 문자열을 이어붙일 수 있습니다.

```dart
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
    'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

여러줄 문자열을 만드는 다른 방법은 작은따옴표 또는 큰따옴표를 세 번 사용하는 것입니다.

```dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

`r` 접두사를 붙여 "raw" 문자열을 만들 수 있습니다.

```dart
var s = r'In a raw string, not even \n gets special treatment.';
```

문자열에서 유니코드 문자열을 표현하는 방법은 [룬](https://www.dartlang.org/guides/language/language-tour#runes)을 참고하십시오.

리터럴 문자열은, 어떠한 보간된 표현식이 null이나 숫자, 문자열, 또는 불리언 값을 내는 컴파일 타임 상수인 한, 컴파일 타임 상수입니다.

```dart
// const 문자열 안에서 동작합니다.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// const 문자열 안에서 작동하지 않습니다.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```

문자열을 사용하는 더 많은 정보는 [문자열 및 정규표현식](https://www.dartlang.org/guides/libraries/library-tour#strings-and-regular-expressions)에서 참고하십시오.

### 불리언

불리언 값을 표현하기 위해 Dart는 `bool`이라는 이름의 타입을 가지고 있습니다. 오직 두 개의 오브젝트만이 bool 타입을 갖습니다. 불리언 리터럴 `true`와 `false`가 그것이며, 둘 모두 컴파일 타임 상수입니다.

Dart의 타입 안전은 `if (불리언이_아닌_값)`이나 `assert (불리언이_아닌_값)`을 사용할 수 없다는 것을 의미합니다. 대신 다음과 같이 명시적으로 값을 검증하십시오.

```dart
// 빈 문자열 검증
var fullName = '';
assert(fullName.isEmpty);

// 0 검증
var hitPoints = 0;
assert(hitPoints <= 0);

// null 검증
var unicorn;
assert(unicorn == null);

// NaN 검증
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```

### 리스트

아마 거의 모든 프로그래밍 언어에서 가장 일반적인 컬렉션은 *배열*, 또는 오브젝트의 순서 있는 그룹일 것입니다. Dart에서 배열은 [List](https://api.dartlang.org/stable/dart-core/List-class.html) 오브젝트이므로, 대부분의 사람들은 *리스트*라고 부릅니다.

Dart의 리스트 리터럴은 JavaScript의 배열 리터럴처럼 보입니다.

```dart
var list = [1, 2, 3];
```

> **알아두기** 분석기는 `list`가 `List<int>` 타입을 가지고 있다고 추론합니다. 리스트에 정수가 아닌 오브젝트를 추가하려고 한다면 분석기나 런타임은 에러를 일으킵니다. [타입 추론](https://www.dartlang.org/guides/language/sound-dart#type-inference)을 참고하십시오.

리스트는 0 기반 인덱싱을 사용합니다. 0은 첫 번째 요소의 인덱스이고 `list.length - 1`은 마지막 요소의 인덱스입니다. JavaScript처럼 리스트의 길이를 얻고 리스트 요소를 참조할 수 있습니다.

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

컴파일 타임 상수 리스트를 만들기 위해 리스트 리터럴 이전에 `const`를 추가하십시오.

```dart
var constantList = const [1, 2, 3];
// constantList[1] = 1; // 이 코드는 에러를 발생시킵니다.
```

List 타입은 리스트를 다루기 위한 많은 손쉬운 메소드를 가지고 있습니다. 리스트에 대한 더 많은 정보는 [제네릭](https://www.dartlang.org/guides/language/language-tour#generics)과 [컬렉션](https://www.dartlang.org/guides/libraries/library-tour#collections)을 참고하십시오.

### 맵

일반적으로 맵은 키와 값으로 구성된 오브젝트입니다. 키와 값은 어떠한 타입의 오브젝트라도 될 수 있습니다. 각각의 *키*는 오직 한 번만 발생하지만, 같은 *값*을 여러 번 사용할 수 있습니다. Dart의 맵에 대한 지원은 맵 리터럴과 [Map](https://api.dartlang.org/stable/dart-core/Map-class.html) 타입에 의해 제공됩니다.

맵 리터럴을 사용하여 생성하는 Dart 맵에 대한 예제입니다.

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

> **알아두기** 분석기는 `gifts`를 `Map<String, String>` 타입으로 추론하고, `nobleGases`를 `Map<int, String>` 타입으로 추론합니다. 이들에게 잘못된 타입의 값을 추가하려 한다면, 분석기나 런타임은 에러를 발생시킵니다. [타입 추론](https://www.dartlang.org/guides/language/sound-dart#type-inference)을 참고하십시오.

Map 생성자를 사용하여 같은 오브젝트를 만들 수 있습니다.

```dart
var gifts = Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

> **알아두기** `Map()` 대신 `new Map()`을 확인할 것이라 기대했을 것입니다. Dart 2에서부터 `new` 키워드는 선택적입니다. [생성자 사용하기](https://www.dartlang.org/guides/language/language-tour#using-constructors)를 참고하십시오.

JavaScript와 같은 방법으로 키-값 쌍을 맵에 추가할 수 있습니다.

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // 키-값 쌍 추가하기
```

JavaScript와 같은 방법으로 맵에서 값을 가져올 수 있습니다.

```dart
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```

맵에 없는 키를 찾는다면 null을 반환받을 것입니다.

```dart
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```

`.length`를 사용하여 맵에 있는 키-값 쌍의 숫자를 얻을 수 있습니다.

```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```

컴파일 타임 상수 맵을 만들기 위해 맵 리터럴 이전에 `const`를 추가하십시오.

```dart
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
// constantMap[2] = 'Helium'; // 이 코드는 에러를 발생시킵니다.
```

맵에 관한 더 많은 정보는 [제네릭](https://www.dartlang.org/guides/language/language-tour#generics)과 [맵](https://www.dartlang.org/guides/libraries/library-tour#maps)을 참고하십시오.

### 룬*Runes*

Dart에서 룬은 문자열의 UTF-32 코드의 포인트들입니다.

유니코드는 전세계의 쓰기 시스템에서 사용되는 각각의 문자, 숫자, 심볼에 대한 유일한 숫자 값을 정의합니다. Dart의 문자열이 UTF-16 코드 단위의 시퀀스이기 때문에, 32비트의 유니코드 값을 문자열 내에 표현하려면 특별한 구문이 필요합니다.

유니코드 코드 포인트를 표현하는 일반적인 방법은 `\uXXXX`입니다. XXXX는 네 개의 16진수 값입니다. 예를 들어 하트 문자는 `\u2665`입니다. 네 개 이하 또는 이상의 16진수를 지정하기 위해 값을 중괄호 안에 위치시킵니다. 예를 들어 웃고 있는 이모지는 `\u{1f600}`입니다.

[String](https://api.dartlang.org/stable/dart-core/String-class.html) 클래스는 룬 정보를 추출하기 위해 사용할 수 있는 몇 가지 프로퍼티를 가지고 있습니다. `codeUnitAt`과 `codeUnit` 프로퍼티는 16비트 코드 단위를 반환합니다. 문자열의 룬을 얻기 위해 `runes` 프로퍼티를 사용하십시오.

다음의 예시는 룬, 16비트 코드 단위, 32비트 코드 포인트 간의 관계를 묘사합니다.

```dart
main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}
/*
👏
[55357, 56399]
[128079]
♥  😅  😎  👻  🖖  👍
👏
[55357, 56399]
[128079]
♥  😅  😎  👻  🖖  👍
*/
```

> **알아두기** 리스트 작업을 사용하여 룬을 다룰 때 주의하십시오. 이 접근법은 특정 언어, 문자 집합, 연산에 따라 쉽게 부서질 수 있습니다. 스택오버플로우에 있는 [Dart에서 String을 어떻게 반전시킵니까?](http://stackoverflow.com/questions/21521729/how-do-i-reverse-a-string-in-dart)를 참고하십시오.

### 심볼*Symbols*

[Symbol](https://api.dartlang.org/stable/dart-core/Symbol-class.html) 오브젝트는 Dart 프로그램에서 선언된 연산자나 식별자를 나타냅니다. 심볼을 사용할 필요가 전혀 없을 수 있으나, 이름으로 식별자를 참조하는 API에는 매우 귀중한 것입니다. 축소는 식별자 심볼이 아닌 식별자 이름을 바꾸기 때문입니다.

식별자에 대한 심볼을 얻기 위해 심볼 리터럴을 사용하십시오. `#` 기호 다음에 식별자가 오는 형태입니다.

```dart
#radix
#bar
```

심볼 리터럴은 컴파일 타임 상수입니다.

## 함수

Dart는 본질적으로 객체지향 언어입니다. 그래서 심지어 함수도 오브젝트이고 [Function](https://api.dartlang.org/stable/dart-core/Function-class.html)이라는 타입을 갖습니다. 이것은 함수가 변수에 할당되거나 다른 함수로 인자로서 넘겨질 수 있다는 것을 의미합니다. 또한 Dart의 클래스의 인스턴스를 마치 함수인 것처럼 호출할 수 있습니다. [호출 가능한 클래스](https://www.dartlang.org/guides/language/language-tour#callable-classes)를 참고하십시오.

함수를 구현하는 예시입니다.

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

Effective Dart가 [public API에 대한 타입 지정](https://www.dartlang.org/guides/language/effective-dart/design#prefer-type-annotating-public-fields-and-top-level-variables-if-the-type-isnt-obvious)을 추천하고 있을지라도, 타입을 생략하더라도 함수는 여전히 동작합니다.

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

오직 하나의 표현식만을 포함하는 함수에서 속기 구문을 사용할 수 있습니다.

```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

`=> 표현식` 구문은 `{ return 표현식; }`의 약칭입니다. `=>` 표기법은 *화살표* 구문이라고 때때로 언급됩니다.

> **알아두기** *선언문*이 아닌, 오직 *표현식*만이 화살표(=>)와 세미콜론(;) 사이에 나타날 수 있습니다. 예를 들어 [if 문](https://www.dartlang.org/guides/language/language-tour#if-and-else)은 거기에 들어갈 수 없으나 [조건 표현식](https://www.dartlang.org/guides/language/language-tour#conditional-expressions)은 들어갈 수 있습니다.

함수는 두 가지 종류의 매개변수, 필수와 선택 매개변수를 가질 수 있습니다. 필수 매개변수는 제일 먼저 나열되며, 이후에 선택 매개변수가 따라옵니다. 이름 있는 선택 매개변수 또한 `@required`로 마크될 수 있습니다.

### 선택 매개변수

선택 매개변수는 

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

- 예외는 기대하지 않은 무언가가 일어난 것을 가리키는 에러.
- 예외가 처리되지 않으면 일반적으로 프로그램이 종료된다.
- Dart의 예외는 모두 확인되지 않은 예외이므로 메소드는 어떤 예외를 던질지 선언할 수 없으며, 어떠한 예외를 캐치할 필요도 없다.
- `Exception` 타입 및 `Error` 타입을 제공한다. 두 오브젝트 뿐만 아니라 `null`이 아닌 오브젝트도 예외로서 던질 수 있다.

### Throw

- `throw` 키워드를 사용하여 예외를 던진다.
- `Error`나 `Exception`을 구현한 타입을 던지는 것이 좋다.

```dart
throw FormatException("Expected at least 1 section");
throw "Out of llamas!";
```

### Catch

- `try` 키워드를 사용하여 예외 처리문을 작성할 수 있음
  - `on` 키워드는 예외 타입을 특정할 필요가 있을 때 사용
  - `catch` 키워드는 예외 오브젝트가 필요할 때 사용
  - 둘 다 한번에 사용 가능, 둘 중 하나만 사용하는 것도 가능
- 예외를 캐치하여 적절한 처리를 해줄 수 있다.

```dart
try {
    breedMoreLlamas();
} on OutOfLlamasException {
    // OutOfLlamasException 타입의 예외에 대한 처리
} on Exception catch (e) {
    // Exception 타입의 예외에 대한 처리
    // on Exception / catch (e)
} catch (e) {
    // 모든 타입을 캐치하여 처리
}
```

- `catch`에 두 개의 매개변수를 작성할 수 있는데, 이 경우 두 번째 매개변수로 스택 트레이스 (`StackTrace` 오브젝트)가 들어온다.
- `catch` 블록에 `rethrow;`를 명시하여 호출자가 예외를 확인하게 할 수 있다.

### Finally

- 예외처리 블록에서 예외가 던져졌든 던져지지 않았든 항상 실행되는 코드를 작성하는 절
- `try`절에서 예외가 던져졌고 `catch`절에서 예외를 처리하지 못했다면, `finally`절이 실행된 후 예외가 발생한다.

```dart
try {
    breedMoreLlamas();
} catch (e) {
    print("Error: $e");
} finally {
    cleanLlamaStalls();
}
```

## 클래스

- Dart는 클래스 및 믹스인 기반 상속을 사용한 객체 지향 언어.
- 모든 오브젝트는 클래스의 인스턴스이며, 모든 클래스는 `Object`의 자식클래스이다.
- **믹스인 기반 상속**
  - `Object`를 제외하고 (기반 클래스이므로), 모든 클래스가 정확히 하나의 부모 클래스를 가지고 있을지라도, 클래스 몸체는 다중 클래스 계층에서 재사용될 수 있다.

### 클래스 멤버 사용하기

- 오브젝트는 함수 및 데이터로 구성된 *멤버*를 갖는다. (각각 *메소드* 및 *인스턴스 변수*)
- 메소드를 호출하는 것은, 오브젝트에서 그것을 이끌어내는 것*invoke*.
- `.` 연산자로 인스턴스 변수나 메소드를 참조할 수 있음
- `?.` 연산자로 왼쪽 피연산자가 `null`일 때 발생하는 예외를 피할 수 있음. `null`인 경우 아무 일도 일어나지 않는다.

### 생성자 사용하기

- 생성자*constructor*를 사용하여 오브젝트를 생성한다.
- 생성자 이름은 `ClassName` 또는 `ClassName.identifier`이다.
- `new` 키워드는 Dart 2부터 사용하지 않아도 된다. 사용하지 말자.

```dart
var p1 = Point(2, 2);
// 생성자의 매개변수에 맵을 넘겨줌
var p2 = Point.fromJson({"x": 1, "y": 2});
```

- 몇몇 클래스는 상수 생성자*constant constructor*를 제공하며, 생성자 이름 앞에 `const`를 붙인다.

```dart
var a = const ImmutablePoint(1, 1);
```

- `const`로 선언된 변수는 이에 할당된 모든 것들도 상수. 암시적으로 모두 `const` 키워드가 붙어있는 것.

### 오브젝트 타입 가져오기

- 오브젝트의 `runtimeType` 프로퍼티로 가져올 수 있음. `Type` 오브젝트를 반환함.

### 인스턴스 변수

```dart
class Point {
    num x;      // 인스턴스 변수 x. 초기값 null
    num y;      // 인스턴스 변수 y. 초기값 null
    num z = 0;  // 인스턴스 변수 z. 초기값 0
}
```

- 모든 인스턴스 변수는 암시적으로 접근자 메소드를 생성한다. `final`이 아닌 인스턴스 변수는 암시적으로 설정자 메소드 또한 생성한다.
  - 메소드라고는 하지만 Swift의 get set이 모두 있는 연산 프로퍼티라고 생각하면 될듯. `.` 연산자를 통해 변수에 접근하거나 값을 할당할 수 있음

### 생성자

- 그 클래스의 이름과 같은 이름의 함수를 만들어 생성자를 선언한다.

```dart
class Point {
    num x, y;
    Point(num x, num y) {
        this.x = x;
        this.y = y;
    }
}
```

- `this` 키워드는 현재 인스턴스를 참조하며, 위의 경우처럼 이름이 충돌할 때만 사용하고, 나머지 경우에는 사용을 지양하자.
- 인스턴스 변수에 생성자 인자를 할당하는 패턴은 매우 흔하므로, 이에 대한 syntatic sugar가 존재한다.

```dart
class Point {
    num x, y;
    Point(this.x, this.y);
}
```

**기본 생성자**

- 생성자를 선언하지 않으면 기본 생성자가 제공된다.
- 인자가 없으며 부모 클래스의 인자가 없는 생성자를 호출한다.

**생성자는 상속되지 않는다**

- 자식 클래스는 부모 클래스로부터 생성자를 상속받지 않는다. 생성자를 선언하지 않은 자식 클래스는 오직 기본 생성자만을 갖는다.

**이름 있는 생성자*Named constructor***

- 하나 이상의 생성자를 구현하거나 추가적인 명백함을 제공하기 위해 이름 있는 생성자를 사용한다.

```dart
class Point {
    num x, y;
    Point(this.x, this.y);
    // 인자를 받지 않는 named constructor
    Point.origin() {
        x = 0;
        y = 0;
    }
}
```

**부모 클래스에 있는 사용자 정의 생성자 호출하기**

- 부모 클래스가 이름이 없고 인자가 없는 생성자를 가지고 있지 않다면, 부모 클래스에 있는 생성자 중 하나를 명시적으로 호출해 주어야 한다.
  - 생성자 몸체 시작 직전에 `:`을 사용하여 상위 클래스의 생성자를 명시한다.
  - `super` 키워드를 사용하여 부모 클래스를 참조한다.

```dart
class Person {
    String firstName;
    Person.fromJson(Map data) {
        print("In Person"):
    }
}
class Employee extends Person {
    Employee.fromJson(Map data) : super.fromJson(data) {
        print("In Employee");
    }
}
```

- 인자는 함수 호출과 같은 표현식일 수 있다.

**이니셜라이저 리스트**

- 상위 클래스의 생성자를 호출하는 것 이외에, 생성자 몸체가 실행되기 전에 인스턴스 변수를 초기화할 수 있다.
- 콤마로 이니셜라이저를 구분한다.

```dart
Point.fromJson(Map<String, num> json) : x = json["x"], y = json["y"] {
    print("In Point.fromJson(): ($x, $x)");
}
```

- `final` 필드를 세팅할 때 사용될 수 있다.

**생성자 리다이렉트**

- 다른 생성자에 초기화를 위임함
- 리다이렉트하는 생성자의 몸체는 비어 있고, 콜론 뒤에 생성자 호출 코드를 작성한다.

```dart
class Point {
    num x, y;
    Point(this.x, this.y);
    Point.alongXAxis(num x) : this(x, 0);
}
```

**상수 생성자**

- 클래스가 절대 변하지 않는 오브젝트를 생산한다면, 이러한 오브젝트들을 컴파일 타임 상수로 만들 수 있다.
- 생성자를 `const`로 정의하고 모든 인스턴스 변수를 `final`로 선언한다.

**팩토리 생성자**

- 항상 새로운 인스턴스를 생성하지 않는 생성자를 구현할 때 `factory` 키워드를 명시한다.
- 다른 생성자처럼 똑같이 호출한다.

### 메소드

- 오브젝트를 위한 행동을 제공하는 함수.

**인스턴스 메소드**

- 인스턴스 변수 및 `this`에 접근할 수 있다.

**접근자 및 설정자**

- 오브젝트의 프로퍼티에 읽고 쓰기 접근을 제공하는 특별한 메소드
- 각각의 인스턴스 변수는 암시적인 접근자를 가지고 있으며, `final`이 아닌 경우 암시적인 설정자 또한 가지고 있다.
- `get` 및 `set` 키워드를 사용하여 추가적인 구현을 해줄 수 있다.

```dart
class Rectangle {
    num left, top, width. height;
    Rectangel(this.left, this.top, this.width, this.height);
    num get right => left + width;
    set right(num value) => left = value - width;
    num get bottom => top + height;
    set bottom(num value) => top = value - height;
}
```

- 메소드처럼 정의하긴 하였으나 실제 사용할 때는 프로퍼티에 접근하거나 값을 할당하는 문법을 사용한다.

**추상 메소드**

- 인스턴스 접근자 및 설정자 메소드는 추상적일 수 있다.
  - 인터페이스를 정의하나 구현은 다른 클래스의 책임이다.
- 추상 클래스에만 존재할 수 있다.

```dart
abstract class Doer {
    // 추상 메소드 선언
    void doSomething();
}
class EffectiveDoer extends Doer {
    void doSomething() {
        // 실제 구현
    }
}
```

### 추상 클래스

- `abstract` 키워드를 사용하여 추상 클래스를 정의한다.
- 인터페이스를 정의할 때 유용하게 사용된다.
- 추상 클래스의 인스턴스를 만들 수는 없다.

### 암시적 인터페이스

- 모든 클래스는 암시적으로 클래스의 모든 인스턴스 멤버와 그것이 구현하는 어떠한 인터페이스를 포함하는 인터페이스를 정의한다.
  - 생성자는 인터페이스에 포함되지 않는다.
- B 클래스의 구현을 상속받지 않고 B 클래스의 API를 지원하는 A 클래스를 만들고 싶다면, A 클래스는 B 클래스를 구현해야 한다.
- `implements` 키워드를 사용하여 구현할 클래스를 명시할 수 있다.
- 다중 인터페이스 구현을 지원한다.

### 클래스 확장

- `extends` 키워드를 사용하여 자식 클래스를 생성
- `super` 키워드를 사용하여 부모 클래스를 참조

**멤버 재정의**

- 자식 클래스는 인스턴스 메소드, 설정자 및 접근자를 재정의할 수 있음
- `@override` 표기를 사용하여 의도적으로 어떤 멤버를 재정의할 것임을 알리기

**재정의 가능한 연산자**

- 여러 연산자를 재정의할 수 있음
- `!=` 연산자는 재정의할 수 없는데, `==` 연산자의 구현에 의한 syntatic sugar일 뿐이기 때문이다.
  - `e1 != e2` / `!(e1 == e2)`

**noSuchMethod()**

- 코드가 존재하지 않는 메소드나 인스턴스 변수에 접근할 때, `noSuchMethod()` 메소드를 재정의하여 존재하지 않는 것에 접근했다는 것에 대한 사용자 정의 처리를 해줄 수 있음
- 위의 메소드를 정의하지 않고 위와 같은 동작을 한다면 NoSuchMethodError가 발생한다.

### 열거형 타입

- 클래스의 특별한 타입
- 상수값의 고정된 숫자를 나타냄

**열거형 사용하기**

- `enum` 키워드 사용

```dart
enum Color { red, green, blue }
```

- 열거형의 각 값은 `index` 접근자를 가지고 있음. 인덱스를 반환함.
- 열거형의 `values` 상수를 사용하여 열거형의 모든 값의 리스트를 얻을 수 있음
- `switch`문의 조건에 사용할 수 있다.
- 열거형은 명시적으로 인스턴스화될 수 없다.
- 열거형을 상속하거나, 믹스인하거나, 구현할 수 없다.
- C언어의 열거형과 비슷한 느낌. Swift처럼 다양한 기능을 가진 열거형은 제공하지 않는다.

### 믹스인 : 클래스에 기능 추가하기

- 믹스인은 다중 클래스 상속에서 클래스 코드를 재사용하는 방법이다.
- 하나 또는 그 이상의 믹스인 이름 앞에 `with` 키워드를 명시하여 믹스인을 사용한다.

```dart
class Musician extends Performer with Musical {
    // ...
}
class Maestro extends Person with Musical, Aggressive, Demented {
    // ...
}
```

- 믹스인을 구현하기 위해 `Object`를 상속받는 클래스를 생성하고 어떠한 생성자로 선언하지 마라.
- 믹스인이 일반적인 클래스처럼 사용되기 원하지 않는다면, `mixin` 키워드를 사용하여 믹스인을 선언하라.

```dart
mixin Musical {
    // ...
}
```

- 오직 특정 타입만 믹스인을 사용하도록 하기 위해, 필요한 부모 클래스를 `on` 키워드를 사용하여 명시한다.

```dart
mixin MusicalPerformer on Musician {
    // ...
}
```

### 클래스 변수 및 메소드

- `static` 키워드를 사용하여 클래스 변수 및 메소드를 선언한다.

**정적 변수**

- 정적 변수 (클래스 변수)는 사용될 때까지 초기화되지 않는다.

**정적 메소드**

- 정적 메소드 (클래스 메소드)는 인스턴스에서 동작하지 않으며, `this` 또한 사용할 수 없다.
- 흔히 사용되는 유틸리티나 기능을 하는 메소드는 전역 함수로 만드는 것을 고려하라.

## 제네릭

기본 배열 타입인 [List](https://api.dartlang.org/stable/dart-core/List-class.html)에 관한 API 문서를 보게 된다면, 사실 그 타입은 `List<E>`임을 알 수 있습니다. <...> 표기법은 List를 *제네릭* 타입 (또는 *매개변수를 갖는parameterized* 타입)이라는 것을 나타냅니다. 타입은 형식적인 타입 매개변수를 갖습니다. 관습적으로 타입 변수는 E, T, S, K, V와 같은 단일 문자로 된 이름을 갖습니다.

### 왜 제네릭을 사용합니까?

제네리은 타입 안전을 위해 종종 요구되나, 그냥 코드를 실행하게 하는 것보다 더 많은 이점을 가지고 있습니다.

- 적절하게 지정된 제네릭 타입은 더 일반화된 코드의 결과를 가져옵니다.
- 코드 중복을 줄이기 위해 제네릭을 사용할 수 있습니다.

오직 문자열만을 포함하는 리스트를 의도한다면 `List<String>`(문자열 리스트라고 읽음)으로 선언할 수 있습니다. 이러한 방법으로 당신과 동료 프로그래머와 사용하고 있는 툴은 리스트에 문자열이 아닌 것을 할당하는 것이 아마 실수였음을 감지할 수 있습니다. 예제는 다음과 같습니다.

```dart
// 에러
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

제네릭을 사용하는 또다른 이유는 코드 중복을 감소시키기 위해서입니다. 제네릭은 많은 타입 간에 하나의 인터페이스와 구현을 공유하게 하는데, 그럼에도 불구하고 여전히 정적 분석의 이점을 취합니다. 예를 들어 오브젝트를 캐싱하기 위한 인터페이스를 만들었다고 합시다.

```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
```

문자열에 특화된 버전의 인터페이스를 원한다는 것을 알게 되었습니다. 이제 또다른 인터페이스를 만듭니다.

```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```

나중에 숫자에 특화된 버전의 인터페이스를 원할 수 있습니다...

제네릭 타입은 이러한 모든 인터페이스를 만드는 믄제에서 당신을 구해낼 수 있습니다. 대신 타입 매개변수를 취하는 인터페이스 하나를 만들 수 있습니다.

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```

이 코드에서 T는 스탠드인*stand-in* 타입입니다. 개발자가 나중에 정의할 타입으로 생각될 수 있는 플레이스홀더입니다.

### 컬렉션 리터럴 사용하기

List와 Map 리터럴은 매개변수를 가질 수 있습니다. 매개변수를 갖는 리터럴은 이미 보아왔던 리터럴처럼 생겼는데, 괄호를 열기 전 리스트에서의 `<type>`이나 맵에서의 `<keyType, valueType>`를 작성하는 것만 제외하고 말입니다. 여기에 타입이 지정된 리터럴을 사용하는 예제가 있습니다.

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```

### 생성자와 함께 매개변수를 갖는 타입 사용하기

생성자를 사용할 때 하나 또는 그 이상의 타입을 지정하기 위해 클래스 이름 바로 뒤에 있는 꺽쇄(`<...>`) 안에 타입을 넣으십시오.

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = Set<String>.from(names);
```

다음의 코드는 정수 키와 View 타입 값을 갖는 맵을 생성합니다.

```dart
var views = Map<int, View>();
```

### 제네릭 컬렉션과 포함하는 타입

Dart의 제네릭 타입은 구체화*reified*되었습니다. 이는 런타임에 제네릭 타입이 그것들의 타입 정보를 가지고 있다는 것을 의미합니다. 예를 들어 컬렉션의 타입을 테스트할 수 있습니다.

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

> **알아두기** 대조적으로 Java에서의 제니릭은 소거*erasure*를 사용합니다. 이는 제네릭 타입 매개변수가 런타임에서 삭제된다는 것을 의미합니다. Java에서 어떠한 오브젝트가 List인지 테스트할 수 있으나 그것이 `List<String>`인지는 테스트할 수 없습니다.

### 매개변수를 갖는 타입 제한하기

제네릭 타입을 구현할 때 그 매개변수의 타입을 제한하고 싶을 수 있습니다. `extends`를 사용하여 이를 달성할 수 있습니다.

```dart
class Foo<T extends SomeBaseClass> {
  // 구현은 여기에 위치함
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
```

제네릭 인자로 `SomeBaseClass`나 그것의 서브클래스를 사용하는 것은 괜찮습니다.

```dart
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
```

또한 제네릭 인자를 지정하지 않는 것도 괜찮습니다.

```dart
var foo = Foo();
print(foo); // 'Foo<SomeBaseClass>'의 인스턴스
```

`SomeBaseClass`가 아닌 어떠한 타입을 지정하는 것은 에러를 초래합니다.

```dart
// 에러
var foo = Foo<Object>();
```

### 제네릭 메소드 사용하기

초기에 Dart에서의 제네릭 지원은 클래스에 한정되었습니다. *제네릭 메소드generic method*라고 불리는 새로운 구문은 메소드와 함수에서 타입 인자를 사용할 수 있게 합니다.

```dart
T first<T>(List<T> ts) {
  // 어떠한 초기 작업이나 에러 확인을 거침. 그리고 나서...
  T tmp = ts[0];
  // 추가적인 검증이나 처리를 진행함
  return tmp;
}
```

`first`에 있는 제네릭 타입 매개변수(`<T>`)는 타입 인자 `T`를 여러 위치에서 사용할 수 있게 합니다.

- 함수의 반환 타입으로 (`T`)
- 인자의 타입으로 (`List<T>`)
- 지역 변수의 타입으로 (`T map`)

[제네릭 메소드 사용하기](https://github.com/dart-lang/sdk/blob/master/pkg/dev_compiler/doc/GENERIC_METHODS.md)에서 제네릭에 관하여 더 많은 정보를 확인하십시오.

## 라이브러리와 가시성

`import`와 `library` 지시는 모듈화되고 공유 가능한 코드 기반을 만드는 것에 도움을 줄 수 있습니다. 라이브러리는 API를 제공할 뿐만 아니라 프라이버시의 단위가 됩니다. 밑줄(_)로 시작하는 식별자는 오직 라이브러리 안에서만 드러나 있습니다. *모든 Dart 앱은 라이브러리*이며, `library` 지시를 사용하지 않을지라도 그렇습니다.

라이브러리는 패키지를 사용하여 분배될 수 있습니다. [Pub 패키지와 에셋 매니저](https://www.dartlang.org/tools/pub)에서 SDK에 포함된 패키지 매니저인 pub에 관한 정보를 확인하십시오.

### 라이브러리 사용하기

`import`를 사용하여 하나의 라이브러리로부터의 네임스페이스가 또다른 라이브러리의 스코프 안에서 사용되는 방법을 지정하십시오.

예를 들어 Dart 웹앱은 일반적으로 [dart:html](https://api.dartlang.org/stable/dart-html) 라이브러리를 사용합니다. 이는 다음과 같이 가져올 수 있습니다.

```dart
import 'dart:html';
```

`import`가 요구하는 인자는 라이브러리를 지정하는 URI 뿐입니다. 내장 라이브러리에 대하여 URI는 특별한 `dart:` 스킴을 가지고 있습니다. 다른 라이브러리에 대하여 파일 시스템 경로 또는 `package:` 스킴을 사용할 수 있습니다. `package:` 스킴은 pub 툴과 같은 패키지 매니저에 의해 제공된 라이브러리를 지정합니다.

```dart
import 'package:test/test.dart';
```

> **알아두기** *URI*는 uniform resource identifier의 약자입니다. URL(uniform resource locator)는 URI의 일반적인 종류입니다.

### 라이브러리 접두사 지정하기

충돌하는 식별자를 가진 두 개의 라이브러리를 가져온다면 하나 또는 둘 모두에 대해 접두사를 지정할 수 있습니다. 예를 들어 library1과 library2가 모두 Element 클래스를 가지고 있다면 다음과 같은 코드를 작성할 수 있습니다.

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// lib1의 Element 사용
Element element1 = Element();

// lib2의 Element 사용
lib2.Element element2 = lib2.Element();
```

### 라이브러리의 일부분만 가져오기

라이브러리의 일부분만 사용하고 싶다면 선택적으로 라이브러리르 가져올 수 있습니다.

```dart
// foo만 가져옵니다.
import 'package:lib1/lib1.dart' show foo;

// foo를 제외한 모든 이름을 가져옵니다.
import 'package:lib2/lib2.dart' hide foo;
```

### 라이브러리의 지연 로딩

지연 로딩*deferred loading / lazy loading*은 애플리케이션이 필요할 때 라이브러리를 로드할 수 있게 합니다. 지연 로딩을 사용할 수 있는 몇 가지 경우가 여기 있습니다.

- 앱의 초기 시작 시간을 줄이기 위해
- A/B 테스팅을 수행하기 위해 : 예를 들어 어떤 알고리즘의 대안 구현을 시험할 때
- 선택적인 화면과 대화상자 같은, 드물게 사용되는 기능을 로드할 때

라이브러리를 지연 로딩하기 위해 먼저 `deferred as`를 사용하여 라이브러리를 가져와야 합니다.

```dart
import 'package:greetings/hello.dart' deferred as hello;
```

라이브러리가 필요할 때 라이브러리의 식별자를 사용하여 `loadLibrary()`를 호출하십시오.

```dart
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

앞선 코드에서 `await` 키워드는 라이브러리가 로디될 때까지 실행을 중지시킵니다. [비동기 지원](https://www.dartlang.org/guides/language/language-tour#asynchrony-support)에서 `async`와 `await`에 대한 더 많은 정보를 확인하십시오.

라이브러리에서 여러번 `loadLibrary()`를 호출하는 것은 문제가 없습니다. 라이브러리는 오직 한번만 로드됩니다.

지연 로딩을 사용할 때 다음의 사항을 염두에 두십시오.

- 지연된 라이브러리의 상수는 임포트하는 파일에서는 상수가 아닙니다. 기억하십시오. 이 상수들은 라이브러리가 지연된 라이브러리가 로딩될 때까지 존재하지 않습니다.
- 임포트하는 파일에서 지연된 라이브러리에 있는 타입을 사용할 수 없습니다. 대신 지연된 라이브러리와 가져오는 파일 모두에 의해 가져와지는 라이브러리로 인터페이스 타입을 옮기는 것을 고려하십시오.
- Dart는 암시적으로 `deferred as namespace`를 사용하여 정의한 네임스페이스에 `loadLibrary()`를 삽입합니다. `loadLibrary()` 함수는 [Future](https://www.dartlang.org/guides/libraries/library-tour#future)를 반환합니다.

> **Dart VM의 차이** : Dart의 VM은 `loadLibrary()`가 호출되기 전에도 지연된 라이브러리의 멤버에 접근하는 것을 허용합니다. 이 행동은 변할 수 있습니다. 그러니 **현재 VM의 행동에 의존하지 마십시오.** [33118번 이슈](https://github.com/dart-lang/sdk/issues/33118)에서 세부 사항을 확인하십시오.

### 라이브러리 구현하기

라이브러리 패키지를 구현하는 방법에 대한 조언을 구하기 위해 [라이브러리 패키지 생성하기](https://www.dartlang.org/guides/libraries/create-library-packages)를 확인하십시오. 다음의 것들도 포함되어 있습니다.

- 라이브러리 소스 코드를 조직하는 방법
- `export` 지시를 사용하는 방법
- `part` 지시를 사용하는 시점
- `library` 지시를 사용하는 시점

## 비동기 지원

Dart의 라이브러리는 [Future](https://api.dartlang.org/stable/dart-async/Future-class.html)나 [Stream](https://api.dartlang.org/stable/dart-async/Stream-class.html) 오브젝트를 반환하는 함수로 가득 차있습니다. 이러한 함수는 비동기적입니다. 그것들은 입출력과 같은 시간을 오래 소요할 가능성이 있는 작업을 마친 후 반환하는데, 그 작업이 완료되기를 기다리지 않습니다.

`async`와 `await` 키워드가 비동기 프로그래밍을 지원합니다. 이는 동기 코드와 비슷하게 보이는 비동기 코드를 작성하게 해줍니다.

### Future 다루기

완료된 Future의 결과가 필요할 때 두 가지 선택지가 있습니다.

- `async`와 `await` 사용하기
- [라이브러리 투어](https://www.dartlang.org/guides/libraries/library-tour#future)에서 서술되는 Future API 사용

`async`와 `await`를 사용한 코드는 비동기적이나 동기 코드인 것처럼 보입니다. 예를 들어 비동기 함수의 결과를 기다리기 위해 `await`를 사용한 코드가 있습니다.

```dart
await lookUpVersion();
```

`await`를 사용하기 위해 코드는 비동기 함수 안에 있어야 합니다. 비동기 함수는 `async`로 마크되어 있습니다.

```dart
Future checkVersion() async {
    var version = await lookUpVersion();
    // 버전을 가지고 어떠한 것을 수행하기
}
```

> **알아두기** 비동기 함수가 시간이 오래 걸리는 작업을 수행할지라도 그러한 작업을 기다리지 않습니다. 대신 비동기 함수는 첫 번째 `await` 표현식[세부사항](https://github.com/dart-lang/sdk/blob/master/docs/newsletter/20170915.md#synchronous-async-start)과 마주칠 때까지만 실행됩니다. 그리고 나서 Future 오브젝트를 반환하며, `await` 표현식이 완료된 직후의 실행을 재개합니다.

`try`, `catch`, `finally`를 사용하여 `await`를 사용한 코드의 에러를 처리하고 정리할 수 있습니다.

```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // 버전을 찾을 수 없음에 대응하기
}
```

비동기 함수에서 `await`를 여러번 사용할 수 있습니다. 예를 들어 다음의 코드는 함수의 결과를 위해 세 번 기다립니다.

```dart
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

`await 표현식`에서, `표현식`의 값은 일반적으로 Future입니다. 그렇지 않다면 값은 자동으로 Future로 감싸집니다. 이 Future 오브젝트는 오브젝트를 반환한다는 약속을 가리킵니다. `await 표현식`의 값은 반환된 오브젝트입니다. await 표현식은 오브젝트가 사용 가능할 때까지 실행을 중단시킵니다.

**`await`를 사용할 때 컴파일 타임 에러가 발생한다면, `await`를 비동기 함수에서 사용했는지 확인하십시오.** 예를 들어 앱의 `main()` 함수에서 `await`를 사용하려면 `main()`의 구현부는 `async`로 마크되어 있어야 합니다.

```dart
Future main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```

### 비동기 함수 선언하기

비동기 함수는 그 몸체가 `async` 한정자로 마크된 함수입니다.

함수에 `async` 키워드를 추가하여 Future를 반환하도록 합니다. 예를 들어 String을 반환하는 다음의 동기 함수를 고려하십시오.

```dart
String lookUpVersion() => "1.0.0";
```

이것을 비동기 함수로 바꾸면, 예를 들어 미래의 구현이 시간이 오래 걸리는 것일 수 있기 때문에, 반환되는 값은 Future입니다.

```dart
Future<String> lookUpVersion() async => "1.0.0";
```

함수의 구현부는 Future API를 사용할 필요가 없다는 것에 주목하십시오. Dart는 필요하다면 Future 오브젝트를 만듭니다.

함수가 유용한 값을 반환하지 않는다면, 그 반환 타입을 `Future<void>`로 만드십시오.

### 스트림 다루기

Stream으로부터 값을 가져올 필요가 있을 때 두 가지 선택지가 있습니다.

- `async`와 *비동기 for 루프asynchronous for loop*(`await for`) 사용하기
- [라이브러리 투어](https://www.dartlang.org/guides/libraries/library-tour#stream)에 서술된 Stream API 사용하기
  
> **알아두기** `await for`를 사용하기 전, 코드가 깔끔하며 스트림의 결과 모두를 기다리는 것을 정말 원하는지 확실하게 하십시오. 예를 들어 보통 UI 이벤트 리스너에서는 `await for`를 **사용해서는 안됩니다.** UI 프레임워크는 끝없는 이벤트 스트림을 전송하기 때문입니다.

비동기 for 루프는 다음의 형태를 가지고 있습니다.

```dart
await for (varOrType identifier in expression) {
  // 스트림이 값을 낼 때마다 실행됨
}
```

*`표현식`*의 값은 Stream 타입이어야 합니다. 실행은 다음의 순서로 진행됩니다.

- 스트림이 값을 낼 때까지 기다림
- 내어진 값에 대한 변수 세트와 함께, for 루프 몸체를 실행함
- 스트림이 닫혀질 때까지 1와 2를 반복함

스트림 수신을 멈추기 위해 `break`나 `return`문을 사용할 수 있습니다. 이는 루프를 빠져나가고 스트림 구독을 해제합니다.

**비동기 for 루프를 구현할 때 컴파일 타임 에러가 발생한다면, `await for`가 비동기 함수 안에 있는지 확인하십시오.** 예를 들어 앱의 `main()` 함수에서 비동기 for 루프를 사용하기 위해 `main()`의 구현부는 `async`로 마크되어 있어야 합니다.

```dart
Future main() async {
  // ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```

일반적으로 비동기 프로그래밍에 관한 더 많은 정보는 라이브러리 투어의 [dart:async](https://www.dartlang.org/guides/libraries/library-tour#dartasync---asynchronous-programming) 섹션에 있습니다. 또한 [Dart 언어의 비동기 지원: 1단계](https://www.dartlang.org/articles/language/await-async)와 [Dart 언어의 비동기 지원: 2단계](https://www.dartlang.org/articles/language/beyond-async), [Dart 언어 명세](https://www.dartlang.org/guides/language/spec)를 확인하십시오.

## 제네레이터

값의 시퀀스의 지연 생산을 필요로 할 때 *제네레이터 함수*를 사용하는 것을 고려하십시오. Dart는 두 종류의 제네레이터 함수에 대한 내장 지원을 가지고 있습니다.

- **동기** 제네레이터 : **[Iterable](https://api.dartlang.org/stable/dart-core/Iterable-class.html)** 오브젝트를 반환
- **비동기** 제네레이터 : **[Stream](https://api.dartlang.org/stable/dart-async/Stream-class.html)** 오브젝트를 반환

**동기** 제네레이터 함수를 구현하기 위해 함수 몸체를 `sync*`로 마크하고, 값을 전달하기 위해 `yield`문을 사용하십시오.

```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```

**비동기** 제네레이터 함수를 구현하기 위해 함수 몸체를 `async*`로 마크하고, 값을 전달하기 위해 `yield`문을 사용하십시오.

```dart
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

제네레이터가 재귀적이면 `yield*`를 사용하여 성능을 향상시킬 수 있습니다.

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```

[Dart 언어의 비동기 지원: 2단계](https://www.dartlang.org/articles/language/beyond-async)에서 제네레이터에 대한 더 많은 정보를 확인하십시오.

## 호출 가능한 클래스

Dart 클래스가 함수처럼 호출되게 하기 위해 `call()` 메소드를 구현하십시오.

다음의 예제에서 `WannabeFunction` 클래스는 세 개의 문자열을 취하고 그것들을 이어붙이면서 공백으로 각각을 분리시키고, 느낌표를 추가하는 call() 함수를 정의합니다. 

```dart
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out');
}
```

[Dart의 함수 에뮬레이팅](https://www.dartlang.org/articles/language/emulating-functions)에서 클래스를 함수처럼 취급하는 것에 대한 더 많은 정보를 확인하십시오.

## Isolates

대부분의 컴퓨터는, 심지어 모바일 플랫폼일지라도, 멀티 코어 CPU를 가지고 있습니다. 이 모든 코어들의 이점을 취하기 위해 개발자는 전통적으로 동시에 동작하는 공유 메모리 스레드를 사용합니다. 그러나 공유 상태의 동시성은 에러를 내기 쉬우며 복잡한 코드로 이끌 수 있습니다.

스레드 대신 모든 Dart의 코드는 *isolate* 안에서 동작합니다. 각각의 isolate는 그것만의 메모리 힙을 가지고 있습니다. 이는 어떠한 isolate의 상태도 다른 어떠한 isolate에 접근할 수 없다는 것을 보장합니다.

[dart:isolate 라이브러리 문서](https://api.dartlang.org/stable/dart-isolate)에서 더 많은 정보를 확인하십시오.

## Typedef

Dart에서 함수는 오브젝트입니다. 문자열과 숫자도 오브젝트입니다. *typedef* 또는 *함수 타입 별칭function-type alias*는 함수 타입에 이름을 주어 필드와 반환 타입을 선언할 때 사용할 수 있게 합니다. typedef는 함수 타입이 변수에 할당될 때 타입 정보를 유지합니다.

```dart
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);

  // compare가 함수라는 것은 압니다.
  // 하지만 함수의 타입은 무엇입니까?
  assert(coll.compare is Function);
}
```

타입 정보는 `f`가 `compare`에 할당될 때 손실됩니다. `f`의 타입은 `(Object, Object) -> int`입니다. `compare`의 타입이 Function이라고 할지라도 말입니다. 명시적인 이름을 사용하고 타입 정보를 유지하기 위해 코드를 바꾸고 싶다면, 개발자와 툴 모두 그 정보를 사용할 수 있습니다.

```dart
typedef Compare = int Function(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

> **알아두기** 현재 typedef는 함수 타입에 제한되어 있습니다. 우리는 이것이 변화하기를 기대합니다.

typedef는 단순히 별칭이기 때문에, 어떠한 함수의 타입을 검증하는 방법을 제공합니다.

```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```

## 메타데이터

메타데이터를 사용하여 코드에 대한 추가적인 정보를 제공할 수 있습니다. 메타데이터 선언은 `@` 문자로 시작하며, 뒤에는 `deprecated`와 같은 컴파일 타임 상수에 대한 참조 또는 상수 생성자에 대한 호출이 따라옵니다.

모든 Dart 코드에서 두 개의 선언, `@deprecated`와 `@override`가 사용 가능합니다. `@override`를 사용하는 것에 대한 예시는 [클래스 확장하기](https://www.dartlang.org/guides/language/language-tour#extending-a-class)를 참고하십시오. `@deprecated` 선언에 대한 예제는 여기 있습니다.

```dart
class Television {
  /// _Deprecated: 대신 turnOn을 사용하기
  @deprecated
  void activate() {
    turnOn();
  }

  /// TV의 전원을 켬
  void turnOn() {...}
}
```

당신 자신의 메타데이터 선언을 정의할 수 있습니다. 두 개의 인자를 취하는 @todo 선언의 정의에 대한 예시입니다.

```dart
library todo;

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

그리고 @todo 선언을 사용하는 것에 대한 예제입니다.

```dart
import 'todo.dart';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
```

메타데이터는 라이브러리, 클래스, typedef, 타입 매개변수, 생성자, 팩토리, 함수, 필드, 파라미터, 변수 선언, 가져오기 또는 내보내기 지시 이전에 나타날 수 있습니다. 반영을 사용하여 런타임에서 메타데이터를 가져올 수 있습니다.

## 주석

Dart는 한줄 주석, 여러줄 주석, 문서화 주석을 지원합니다.

### 한줄 주석

한줄 주석은 `//`로 시작합니다. `//`와 라인 끝 사이에 있는 모든 것은 Dart 컴파일러에 의해 무시됩니다.

```dart
void main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
```

### 여러줄 주석

여러줄 주석은 `/*`로 시작하고 `*/`로 끝납니다. `/*`와 `*/` 사이에 있는 모든 것은, 그 주석이 문서화 주석이 아니라면, Dart 컴파일러에 의해 무시됩니다. 여러줄 주석은 중첩될 수 있습니다.

```dart
void main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
```

### 문서화 주석

문서화 주석은 `///`이나 `/**`로 시작하는 여러줄 또는 한줄 주석입니다. 연속되는 라인에 `///`를 사용하여 여러줄 문서화 주석의 효과를 낼 수 있습니다.

문서화 주석 안에서 Dart 컴파일러는 대괄호로 둘러싸인 것이 아닌 모든 텍스트를 무시합니다. 대괄호를 사용하여 클래스, 메소드, 필드, 전역 변수, 함수, 매개변수를 참조할 수 있습니다. 대괄호에 있는 이름은 문서화된 프로그램 요소의 lexical scope에서 확정됩니다.

다른 클래스와 인자의 참조가 들어간 문서화 주석에 대한 예시입니다.

```dart
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
class Llama {
  String name;

  /// Feeds your llama [Food].
  ///
  /// The typical llama eats one bale of hay per week.
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
```

생성된 문서에서 `[Food]`는 Food 클래스에 대한 API 문서로의 링크가 됩니다.

Dart 코드를 파싱하여 HTML 문서를 생성하기 위해 SDK의 [문서 생성 도구](https://github.com/dart-lang/dartdoc#dartdoc)를 사용할 수 있습니다. 생성된 문서의 한 예시로 [Dart API 문서](https://api.dartlang.org/stable)를 확인하십시오. 문서를 구조화하는 방법에 대한 조언을 얻기 위해 [Dart 문서화 주석 가이드라인](https://www.dartlang.org/guides/language/effective-dart/documentation)를 참고하십시오.

## 요약

이 페이지는 Dart 언어에서 일반적으로 사용되는 특징을 요약한 것입니다. 더 많은 기능이 구현되어 있으나, 그것들이 존재하는 코드를 부수지 않을 것이라고 예상합니다. [Dart 언어 명세](https://www.dartlang.org/guides/language/spec)와 [Effective Dart](https://www.dartlang.org/guides/language/effective-dart)에서 더 많은 정보를 확인하십시오.

Dart의 코어 라이브러리에 대해서 [Dart 라이브러리로의 투어](https://www.dartlang.org/guides/libraries/library-tour)에서 더 배울 수 있습니다.