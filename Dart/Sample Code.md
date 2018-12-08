# Sample Code

이 컬렉션은 포괄적이지 않습니다. 단지 예제로 배우고 싶어하는 사람들을 위한 언어의 간략한 소개일 뿐입니다. 언어 및 라이브러리 투어를 확인하는 것을 원할 수도 있습니다.

- [Language Tour](https://www.dartlang.org/guides/language/language-tour) : Dart 언어의 예제와 함께하는 포괄적인 투어. 이 페이지에 있는 대부분의 *더 읽어보기* 링크는 Language Tour로 이어집니다.
- [Library Tour](https://www.dartlang.org/guides/libraries/library-tour) : Dart의 코어 라이브러리에 대한 예제 기반 소개. 빌트인 타입, 컬렉션, 날짜 및 시간, 스트림, 그리고 더 많은 것을 사용하는 방법을 확인하십시오.

## Hello World

모든 앱은 `main()` 함수를 가지고 있습니다. 콘솔에 텍스트를 출력하기 위해 `print()` 전역함수를 사용할 수 있습니다.

```dart
void main() {
  print('Hello, World!');
}
```

## 변수*Variables*

Dart의 코드가 타입에 안전할지라도 대부분의 변수에 타입을 명시할 필요는 없습니다. 타입 추론 덕분입니다.

```dart
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};
```

기본값, `final` 및 `const` 키워드, 정적 타입을 포함하여 Dart의 변수에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#variables)

## 흐름 제어문*Control flow statements*

Dart는 일반적인 흐름 제어문을 지원합니다.

```dart
if (year >= 2001) {
  print('21st century');
} else if (year >= 1901) {
  print('20th century');
}

for (var object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
```

`break` 및 `continue`, `switch` 및 `case`, `assert`를 포함하여 Dart의 흐름 제어문에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#control-flow-statements)

## 함수*Functions*

각 함수의 인자와 반환값에 타입을 명시하는 것을 [권장합니다](https://www.dartlang.org/guides/language/effective-dart/design#types).

```dart
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

간단명료한 `=>` (화살표) 구문은 하나의 문장을 포함하는 함수에서 유용합니다. 이 구문은 특히 인자로 익명 함수를 넘길 때 유용합니다.

```dart
flybyObjects.where((name) => name.contains('turn')).forEach(print);
```

위의 코드는 익명 함수 (`where()`의 인자)를 보여줄 뿐만 아니라, 함수를 인자로도 사용할 수 있음을 보여줍니다. `print()` 전역함수는 `forEach()`의 인자로 사용되었습니다.

옵셔널 매개변수, 매개변수 기본값, lexical scope를 포함하여 Dart의 함수에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#functions)

## 주석*Comments*

Dart에서 주석은 보통 `//`으로 시작합니다.

```dart
// This is a normal, one-line comment.

/// This is a documentation comment, used to document libraries,
/// classes, and their members. Tools like IDEs and dartdoc treat
/// doc comments specially.

/* Comments like these are also supported. */
```

문서화 도구가 작동하는 방법을 포함하여 Dart의 주석에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#comments)

## 가져오기*Imports*

다른 라이브러리에 정의된 API에 접근하기 위해 `import`를 사용하십시오.

```dart
// 코어 라이브러리 가져오기
import 'dart:math';

// 외부 패키지로부터 라이브러리 가져오기
import 'package:test/test.dart';

// 파일 가져오기
import 'path/to/my_other_file.dart';
```

라이브러리 접두사, `show` 및 `hide`, `deferred` 키워드를 통한 지연 로딩을 포함하여 Dart의 라이브버리와 가시성에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#libraries-and-visibility)

## 클래스*Classes*

세 개의 프로퍼티, 두 개의 생성자, 하나의 메소드를 갖는 클래스의 예시가 있습니다. 프로퍼티 중 하나는 직접적으로 설정될 수 없으므로 (변수 대신) 접근자 메소드를 사용하여 정의되었습니다.

```dart
class Spacecraft {
  String name;
  DateTime launchDate;

  // 멤버 할당을 위한 syntatic sugar가 첨가된 생성자
  Spacecraft(this.name, this.launchDate) {
    // Initialization code goes here.
  }

  // 기본의 것을 앞에 두는 이름지어진 생성자
  Spacecraft.unlaunched(String name) : this(name, null);

  int get launchYear =>
      launchDate?.year; // 읽기 전용, final이 아닌 프로퍼티

  // 메소드
  void describe() {
    print('Spacecraft: $name');
    if (launchDate != null) {
      int years =
          DateTime.now().difference(launchDate).inDays ~/
              365;
      print('Launched: $launchYear ($years years ago)');
    } else {
      print('Unlaunched');
    }
  }
}
```

`Spacecraft` 클래스를 다음과 같이 사용할 것입니다.

```dart
var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
```

이니셜라이저 리스트, 옵셔널 `new` 및 `const`, 생성자 리다이렉트, `factory` 생성자, 접근자, 생성자, 그리고 더 많은 것을 포함하여 Dart의 클래스에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#classes)

## 상속*Interitance*

Dart는 단일 상속을 지원합니다.

```dart
class Orbiter extends Spacecraft {
  num altitude;
  Orbiter(String name, DateTime launchDate, this.altitude)
      : super(name, launchDate);
}
```

클래스 확장, 옵셔널 `@override` 선언, 그리고 더 많은 것에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#extending-a-class)

## 믹스인*Mixins*

믹스인은 다중 클래스 계층에서 코드를 재사용하는 방법입니다. 다음의 클래스는 믹스인인 것처럼 행동할 수 있습니다.

```dart
class Piloted {
  int astronauts = 1;
  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}
```

클래스에 믹스인의 능력을 추가하려면 클래스를 믹스인과 함께 확장하기만 하면 됩니다.

```dart
class PilotedCraft extends Spacecraft with Piloted {
  // ···
}
```

`Orbiter`는 이제 `describeCrew()` 메소드뿐만 아니라 `astronauts` 필드를 갖게 됩니다.

믹스인에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#adding-features-to-a-class-mixins)

## 인터페이스 및 추상 클래스*Interfaces and abstract classes*

Dart에는 `interface` 키워드가 없습니다. 대신 모든 클래스는 암시적으로 인터페이스를 정의합니다. 그러므로 어떠한 클래스라도 구현할 수 있습니다.

```dart
class MockSpaceship implements Spacecraft {
  // ···
}
```

암시적 인터페이스에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#implicit-interfaces)

추상 클래스를 만들어 각각의 클래스에 의해 확장되거나 구현되도록 할 수 있습니다. 추상 클래스는 추상 메소드를 포함할 수 있으며 추상 메소드의 몸체는 비어 있습니다.

```dart
abstract class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
```

`Describable`을 확장하는 클래스는 `describeWithEmphasis()` 메소드를 갖게 됩니다. 그것은 `describe()` 메소드를 호출하여 확장하는 것의 구현을 호출하게 됩니다.

추상 클래스 및 메소드에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#abstract-classes)

## 비동기*Async*

콜백 지옥을 피하고 코드의 가독성을 높이기 위해 `asnyc` 및 `await`를 사용할 수 있습니다.

```dart
const oneSecond = Duration(seconds: 1);
// ···
Future<void> printWithDelay(String message) async {
  await Future.delayed(oneSecond);
  print(message);
}
```

위의 방법은 아래와 동일합니다.

```dart
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
```

다음 예제에서 확인할 수 있다시피 `async`와 `await`는 비동기 코드를 읽기 쉽게 하는 것을 도와줍니다.

```dart
Future<void> createDescriptions(Iterable<String> objects) async {
  for (var object in objects) {
    try {
      var file = File('$object.txt');
      if (await file.exists()) {
        var modified = await file.lastModified();
        print(
            'File for $object already exists. It was modified on $modified.');
        continue;
      }
      await file.create();
      await file.writeAsString('Start describing $object in this file.');
    } on IOException catch (e) {
      print('Cannot create description for $object: $e');
    }
  }
}
```

스트림을 만드는 훌륭하고 가독성 좋은 수단을 제공하는 `async*`를 사용할 수도 있습니다.

```dart
Stream<String> report(Spacecraft craft, Iterable<String> objects) async* {
  for (var object in objects) {
    await Future.delayed(oneSecond);
    yield '${craft.name} flies by $object';
  }
}
```

비동기 함수, `Futuer`, `Stream`, 비동기 루프(`await for`)를 포함하여 비동기 지원에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#asynchrony-support)

## 예외*Exceptions*

예외를 발생시키기 위해 `throw`를 사용하십시오

```dart
if (astronauts == 0) {
  throw StateError('No astronauts.');
}
```

예외를 캐치하기 위해 `on`이나 `catch`, 또는 둘 모두와 함께 `try`문을 사용하십시오.

```dart
try {
  for (var object in flybyObjects) {
    var description = await File('$object.txt').readAsString();
    print(description);
  }
} on IOException catch (e) {
  print('Could not describe object: $e');
} finally {
  flybyObjects.clear();
}
```

위의 코드는 비동기적임을 알아두십시오. `try`는 동기적인 코드와 비동기 함수에 있는 코드 모두에서 동작하고 있습니다.

스택 트레이스, `rethrow`, Error와 Exception의 차이를 포함하여 예외에 관하여 [더 읽어보기](https://www.dartlang.org/guides/language/language-tour#exceptions)

## 다른 주제*Other topics*

더욱더 많은 코드가 [Language Tour](https://www.dartlang.org/guides/language/language-tour)와 [Library Tour](https://www.dartlang.org/guides/libraries/library-tour)에 있습니다. [API 레퍼런스](https://api.dartlang.org/)도 참고하십시오. 종종 예제를 포함합니다.