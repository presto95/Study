# Adding Native Code to our Flutter App

## Module Introduction

[문서](https://flutter.dev/docs/development/platform-integration/platform-channels)

네이티브에 특화된 기능을 구현하기 위해 네이티브 코드를 작성할 필요가 있다.

안드로이드 : Java / Kotlin

iOS : Swift / Objective-C

Flutter와 네이티브가 상호작용하는 방식을 이해한다.

## Understanding the Communication Between Flutter & Native Code

MethodChannel을 사용하여 Flutter와 네이티브 간 소통이 이루어진다.

```dart
// main.dart

// MethodChannel 객체를 만든다. 인자는 유일해야 하므로 일반적으로 패키지 이름을 넘겨준다.
final _platformChannel = MethodChannel("com.presto.flutter_course");
// MethodChannel을 사용하여 메소드를 호출한다.
// 메소드의 이름은 문자열의 형태로 넘겨준다.
// 제네릭 타입을 명시하여 반환형을 알려준다.
final result = await _platformChannel.invokeMethod<int>("getBattery");
```

문서를 보자.