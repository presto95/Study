# Diving Into the "main.dart" File

Flutter에서 `main.dart` 파일은 무조건 존재해야 한다. `main` 전역 함수도 존재해야 한다.

# Widgets in Flutter - Theory

Widget은 기본적으로 블록, UI 컴포넌트 등을 만들 때 사용한다. 화면 그 자체도 Widget이다.

로직도 포함할 수 있다.

Flutter App == Widget Tree

# Creating a Widget

`import`로 외부 모듈 가져오기 (`package:flutter/material.dart`)

# Adding the "Build" Method

`Widget build(BuildContext context)`

- 반환 타입 `Widget` / 인자는 하나 `BuildContext`
  - `context` : 애플리케이션에 대한 메타 데이터를 포함하는 오브젝트
- 무엇을 화면에 그릴지를 나타냄

**Widget은 `build` 메소드에서 또다른 Widget을 항상 반환한다.**

`MaterialApp`은 앱의 최상단에 위치할 수 있는 Widget. 앱의 테마 설정 등.

# Adding the Scaffold

`MaterialApp` 만으로는 아무것도 표시할 수 없다. 앱 전체에서 사용할 Wrapper를 명시해 주어야 한다.

일반적으로 `Scaffold` 객체를 할당한다. app bar, drawer, floating action button 등을 추가할 수 있는 컨테이너의 역할.

---

### Dart

named argument : 이름 있는 인자 | positional argument : 이름 없는 인자.

### VSCode

shift option F : Re-Format

option space : 자동 완성 리스트 보여주기 (임의 설정함)

---

```dart
void main() {
  runApp(MyApp());
}
```

`main` 함수에서 `runApp` 메소드를 호출한다.

# Diving Deeper Into the Syntax

단일 표현식을 반환하는 함수의 경우 `=>` 를 사용하여 함수를 구현할 수 있다.

```dart
void main() => runApp(MyApp());
```

# Adding Cards & Images

이미지 등의 리소스를 담는 폴더의 이름은 `assets` 정도로 해주면 좋다.

이를 사용하기 위해서는 `pubspec.yaml` 파일에서 디렉토리 경로를 명시해주어야 한다.

```yaml
assets:
  - assets/food.jpg
```

Material에 있는 `Card` 는 drop shadow 효과가 있는 컨테이너의 역할을 할 수 있다. `child` 프로퍼티에 자식 위젯을 추가할 수 있다.

iOS의 `UIStackView`와 비교 가능한 `Column`이나 `Row` 같은 위젯은 `children` 프로퍼티에 여러 개의 위젯을 추가할 수 있다.

# Diving Into the Official Docs

공식 문서에서 Widget들에 대해 참고하기

머터리얼 쪽이 예제도 많으므로 빠른 개발을 위해서는 Material Components를 우선적으로 참고하기

# Adding a Button

---

### VSCode

ctrl shift R을 눌러 리팩토링 가능. Padding을 추가하거나, Column이나 Row에 빠르게 감쌀 수 있게 해준다.

---

Scaffold 위젯의 body 프로퍼티에 Card를 바로 넘겨주면 Card가 화면 전체를 차지하게 되는데, 이를 Column으로 감싸주면 기대하는 컨텐츠 크기만큼만 Card가 공간을 차지하게 된다.

Widget을 Container에 감싸 마진, 패딩, 색상 등을 설정할 수 있다.

# Creating a Stateful Widget

`build` 메소드는 앱이 최초에 로드될 때, 데이터가 변경될 때 호출된다.

이러한 작업은 Stateless Widget에서는 불가능하다. 정적이기 때문이다.

Stateless Widget은 단순히 위젯 트리를 만드는 역할을 할 뿐이다. 내부 데이터로 어떠한 작업을 수행할 수 없다. `build` 메소드를 다시 호출하지 않는다.

---

### Dart

언더스코어를 변수명이나 클래스명 앞에 붙여 private 접근 수준이라는 것을 명시한다.

Dart에는 접근 수준 지정자가 따로 없다.

### VSCode

command . 을 눌러 오류가 발생한 라인이 제안하는 것을 빠르게 확인할 수 있다.

---

StatefulWidget 클래스를 상속받기 위해 `createState` 메소드를 재정의해야 한다. 이는 해당 위젯의 상태를 관리하는 클래스를 반환하도록 한다.

# Managing Data Inside Stateful Widgets

State를 상속받은 클래스에 프로퍼티를 선언하여 내부 데이터를 정의한다.

---

### Dart

Dart의 배열(리스트)는 `List` 클래스가 제공하며, 제네릭 타입에 요소의 타입을 지정한다.

Dart의 `List<String>` 은 Swift의 `[String]` (`Array<String>`)와 비교 가능하다.

---

# Adding the Stateful Widget & Lists

데이터가 변화할 때 Flutter가 알아서 `build` 메소드를 호출해 주지는 않는다.

데이터에 변화가 일어난 시점에 Flutter가 그것을 다시 보고 렌더링하게 하도록 해야 한다.

이는 `setState` 메소드를 통해 실현 가능하다. 

예를 들어 버튼의 `onPressed` 프로퍼티에 넘겨줄 익명 함수에 `setState` 메소드를 구현해 주어 데이터에 변화를 주고 렌더링을 다시 하게 할 수 있다.

# Splitting our Code Up

코드를 잘 분리하고 여러 개의 파일에 위치시켜 가독성과 유지보수성을 최대화해야 한다.

---

### Dart

파일의 이름은 소문자와 언더스코어를 사용하여 작명한다.

final 키워드를 사용하여 immutable value임을 나타낸다.

각각의 파일에서 접근하려는 파일이나 패키지를 매번 import 해주어야 한다.

```dart
class Product extends StatelessWidget {
  
  final List<String> _products;
 
  // 아래 두 개의 Constructor는 같은 역할을 한다.
  Product(List<String> products) : _products = products;
  
  Product(this._products);
}
```

---

# Creating the "Product Manager" Widget

