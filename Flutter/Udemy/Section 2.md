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

`Column`이나 `Row` 같은 위젯은 `children` 프로퍼티에 여러 개의 위젯을 추가할 수 있다. iOS에서 `UIStackView`와 비교할 수 있다.