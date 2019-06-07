# Diving Into the Basics & Understanding Widgets

## Diving Into the "main.dart" File

Flutter에서 `main.dart` 파일은 무조건 존재해야 한다. `main` 전역 함수도 존재해야 한다.

## Widgets in Flutter - Theory

Widget은 기본적으로 블록, UI 컴포넌트 등을 만들 때 사용한다. 화면 그 자체도 Widget이다.

로직도 포함할 수 있다.

Flutter App == Widget Tree

## Creating a Widget

`import`로 외부 모듈 가져오기 (`package:flutter/material.dart`)

## Adding the "Build" Method

`Widget build(BuildContext context)`

- 반환 타입 `Widget` / 인자는 하나 `BuildContext`
  - `context` : 애플리케이션에 대한 메타 데이터를 포함하는 오브젝트
- 무엇을 화면에 그릴지를 나타냄

**Widget은 `build` 메소드에서 또다른 Widget을 항상 반환한다.**

`MaterialApp`은 앱의 최상단에 위치할 수 있는 Widget. 앱의 테마 설정 등.

## Adding the Scaffold

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

## Diving Deeper Into the Syntax

단일 표현식을 반환하는 함수의 경우 `=>` 를 사용하여 함수를 구현할 수 있다.

```dart
void main() => runApp(MyApp());
```

## Adding Cards & Images

이미지 등의 리소스를 담는 폴더의 이름은 `assets` 정도로 해주면 좋다.

이를 사용하기 위해서는 `pubspec.yaml` 파일에서 디렉토리 경로를 명시해주어야 한다.

```yaml
assets:
  - assets/food.jpg
```

Material에 있는 `Card` 는 drop shadow 효과가 있는 컨테이너의 역할을 할 수 있다. `child` 프로퍼티에 자식 위젯을 추가할 수 있다.

iOS의 `UIStackView`와 비교 가능한 `Column`이나 `Row` 같은 위젯은 `children` 프로퍼티에 여러 개의 위젯을 추가할 수 있다.

## Diving Into the Official Docs

공식 문서에서 Widget들에 대해 참고하기

머터리얼 쪽이 예제도 많으므로 빠른 개발을 위해서는 Material Components를 우선적으로 참고하기

## Adding a Button

---

### VSCode

ctrl shift R을 눌러 리팩토링 가능. Padding을 추가하거나, Column이나 Row에 빠르게 감쌀 수 있게 해준다.

---

Scaffold 위젯의 body 프로퍼티에 Card를 바로 넘겨주면 Card가 화면 전체를 차지하게 되는데, 이를 Column으로 감싸주면 기대하는 컨텐츠 크기만큼만 Card가 공간을 차지하게 된다.

Widget을 Container에 감싸 마진, 패딩, 색상 등을 설정할 수 있다.

## Creating a Stateful Widget

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

## Managing Data Inside Stateful Widgets

State를 상속받은 클래스에 프로퍼티를 선언하여 내부 데이터를 정의한다.

---

### Dart

Dart의 배열(리스트)는 `List` 클래스가 제공하며, 제네릭 타입에 요소의 타입을 지정한다.

Dart의 `List<String>` 은 Swift의 `[String]` (`Array<String>`)와 비교 가능하다.

---

## Adding the Stateful Widget & Lists

데이터가 변화할 때 Flutter가 알아서 `build` 메소드를 호출해 주지는 않는다.

데이터에 변화가 일어난 시점에 Flutter가 그것을 다시 보고 렌더링하게 하도록 해야 한다.

이는 `setState` 메소드를 통해 실현 가능하다. 

예를 들어 버튼의 `onPressed` 프로퍼티에 넘겨줄 익명 함수에 `setState` 메소드를 구현해 주어 데이터에 변화를 주고 렌더링을 다시 하게 할 수 있다.

## Splitting our Code Up

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

## Creating the "Product Manager" Widget

UI 설계에 계층적 구조를 적용, Manager 클래스가 최종 형태의 UI를 만들어내게끔 한다.

어떻게 UI 요소를 분리하고 재사용할 수 있게 할지 생각해야 한다.

일반적으로 Stateless Widget을 사용하고, 데이터를 관리하고 변경할 필요가 있는 것에는 Stateful Widget을 사용한다.

## Passing Data to Stateful Widget

외부에서 데이터 전달하기

StatefulWidget 클래스를 상속받는 클래스는 immutable하며, State를 상속받은 클래스는 mutable하다.

State 클래스는 `widget` 프로퍼티를 통해 해당 클래스의 제네릭 타입에 명시된 위젯 클래스의 프로퍼티와 메소드에 접근할 수 있도록 한다.

State 클래스를 상속받는 클래스는 `initState` 메소드를 재정의하여 초기 상태를 설정할 수 있다.

`initState` 메소드는 `build` 메소드 이전에 호출된다.

## initState() and super.initState()

```dart
@override
void initState() {
  super.initState();
  _products.add(widget.startingProduct);
}
```

위와 같이 먼저 상위 클래스의 메소드를 호출한 후 구현하는 것을 추천한다.

## Understanding Lifecycle Hooks

Stateless Widget의 경우 위젯에 데이터를 주입하여 UI를 렌더링한다. 데이터는 외부에 의해 변경될 수 있고 이 변화에 따라 다시 렌더링되어 UI를 갱신한다.

Stateful Widget의 경우 Stateless Widget에 더하여 내부 상태를 가질 수 있다. 이는 외부 또는 내부에 의한 데이터 변화에 따라 다시 렌더링되어 UI를 갱신한다.

### Life Cycle

- Stateless Widget
  1. 생성자 호출
  2. `build()` 호출
     - 외부 데이터의 변화에 따라 여러 번 호출될 수 있음

> Stateless Widget의 Constructor -> build()

- Stateful Widget

  1. Widget의 생성자 호출
  2. Widget의 `createState()` 호출
  3. State의 `initState()` 호출
  4. State의 `build()` 호출
     - `setState()` 에 의하여 다시 렌더링해야 하는 경우 호출될 수 있음. 버튼 탭 이벤트나 네트워크 요청이 종료된 후 등 `setState()` 를 호출할 수 있음

  - `didUpdateWidget()`
    - 외부 데이터에 의해 변화가 일어난 경우 호출되며, 이후 `build()` 가 호출됨
    - hot reload되는 경우에도 호출됨 

> Stateful Widget의 Constructor -> createState() 이후 State의 initState() -> build()

Widget의 생명주기를 이해하는 것은 iOS에서 `UIViewController` 의 생명주기를 이해하는 것처럼 중요한 부분일 것이므로 충분히 이해하도록 하자.

## Diving Into Google's Material Design

Flutter에 내장된 Material Design System이 적용된 Widget을 사용하여 자유롭게 커스터마이징할 수 있으나 유려한 디자인을 취할 수 있다.

- MaterialApp 클래스의 `theme` 프로퍼티에 테마 정보를 넘겨줄 수 있으며, 일반적으로 ThemeData 클래스를 사용한다.
  - ThemeData 클래스의 `primarySwatch` 프로퍼티에 미리 정의된 색상 집합을 넘겨줄 수 있다.
  - 해당 테마를 외부에서는 `Theme.of(context)` 를 사용하여 접근할 수 있다.
  - 테마와 관련된 여러 프로퍼티에 값을 할당하여 앱 전체의 테마 설정을 할 수 있다.

## Understanding Additional Dart Features

final과 const의 차이

```dart
class Product {
  final String product;
  Product(this.product);
}

Product("Product");

///////////

class Product {
  final String product;
  Product({this.product});
}

Product(product: "Product");
```

Dart에서 생성자를 정의하고 사용할 때의 syntactic sugar를 이해하기.

매개 변수 기본값을 사용할 수 있음

- 중괄호 안에 위치하는 named argument의 경우
  - 다른 언어에서 하는 것처럼 매개 변수 기본값 정의해주기
  - `void enableFlags({bool bold = false, bool hidden = false}) { ... }`
    - `enableFlags(bold: true, hidden: true,)`
- 일반적인 positional argument의 경우
  - 대괄호 안에 위치시킴
  - `String say(String from, String msg, [String device = "carrier pigeon"])`
    - `say("From", "Msg", "Device")`
  - 매개 변수에 값이 넘겨지지 않은 경우 null

공식 문서에서 Dart 언어의 사용법 확인하기

## Passing Data Up

**위젯 트리의 상위 위젯에서 하위 위젯의 상태를 관리하도록 한다.** 이 때 상위 위젯은 Stateful하며, 나머지는 Stateless하다.

상위 위젯은 하위 위젯에 상태를 전파할 필요가 있다.

`Function` 이라는 추상 클래스가 존재하며, 모든 함수 타입의 기반 클래스이다.

**Pass Data Up** / **Pass Reference Down**

상위 위젯에 버튼(하위 위젯)을 설정하고, 버튼이 눌렸을 때 호출할 콜백 메소드를 등록한 형태로 생각하기.

## Understanding "const" & "final"

### final

final로 지정된 변수는 한번 초기화된 이후 **새로운 값으로 다시 할당될 수 없다.**

하지만 Dart에서 모든 타입은 클래스이고 참조 타입이므로, 메모리에 존재하는 객체를 변경하는 것은 가능하다.

### const

final의 행위에 더하여 모든 경우에 대한 변경이 불가능하다.

`final List<String> _products = const [];` 의 코드가 있을 때 `_products` 에 `add` 와 같은 메소드를 호출하면 런타임 에러가 발생한다. 초기화된 빈 배열은 변경이 불가능하기 때문이다.

`final List<String> _products = [];` 의 코드가 있을 때는 `add` 와 같은 메소드를 호출할 수 있다.

어떠한 값이 절대 변하지 않을 것임을 명시하기 위해 r-value에 `const` 키워드를 사용하여 할당한다.

l-value에 `final`, r-value에 `const` 를 사용하여 해당 변수에는 새로운 객체를 할당할 수 없고, 할당된 객체를 변경할 수도 없게 한다.

## Dart Types, Syntax & Core Features

- Dart의 모든 것은 오브젝트, 참조 타입이다.
- Dart는 강타입 언어다.
- `num` 은 `int` 나 `double` 같은 숫자 타입의 기반 클래스다.
- 텍스트는 `String` 타입.
- 불리언 값은 `bool` 타입.
- 리스트 `List<T>`
- 맵은 Swift의 딕셔너리를 생각하기. 중괄호를 사용.

## Wrap Up

- Flutter는 여러 개의 Widget에 대한 것이다.
  - 내장 또는 커스텀 위젯을 사용하여 UI를 구성한다.
  - Stateless Widget은 데이터를 입력으로만 취한다. 내부적으로 데이터를 조작하지 않는다. 새로운 위젯 트리를 렌더링한다.
  - Stateful Widget은 외부 데이터와 내부 데이터를 모두 취할 수 있으며, 내부 데이터는 state라고도 불린다.
    - 내부 데이터는 State 객체가 관리한다.
  - 외부 또는 내부 데이터의 변화는 다시 렌더링하는 결과를 낳는다. 이는 라이프 사이클에 따라 진행된다.
- Dart와 Flutter 간 관계
  - Dart는 Flutter가 사용하는 프로그래밍 언어
  - Flutter는 Flutter SDK와 Dart Framework를 말한다.
- 데이터 전달 / 위젯 생명주기
  - 생성자를 정의하여 데이터 전달
  - Stateful Widget의 경우 State에서 `widget` 프로퍼티를 사용하여 Widget의 프로퍼티에 접근 가능하다.
  - Stateful Widget의 생명주기
    - initState : 위젯과 그 상태가 최초로 만들어질 때 호출됨
    - didUpdateWidget : 위젯이 새로운 외부 데이터를 받을 때 호출됨