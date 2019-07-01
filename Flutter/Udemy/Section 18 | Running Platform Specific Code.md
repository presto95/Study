# Running Platform Specific Code

## Module Introduction

Material Design System을 활용하여 만들어진 앱은 안드로이드 사용자에게 익숙하지만 iOS의 사용자들에게는 익숙하지 않을 수 있다.

런타임에서 Flutter 앱이 실행 중인 플랫폼을 체크하고 플랫폼에 최적화된 사용자 경험을 제공하기 위한 방법을 알아본다.

## Understanding Material Design & Cupertino

Cupertino 패키지에서 iOS에 특화된 형태의 위젯을 사용할 수 있다.

일반적으로 Material 패키지를 사용하여 앱을 구현하고, iOS 스타일의 위젯이 필요할 때 가져다 사용할 수 있다.

Cupertino 패키지를 Material 패키지의 대안 격으로 생각하면 안된다. Material 패키지에는 Flutter 앱의 구동에 필요한 중요한 기능을 담고 있으며, Cupertino 패키지는 iOS 스타일의 위젯을 제공할 뿐이다.

## Detecting the Platform

런타임에서 실행 중인 플랫폼을 확인하는 방법

- `Theme.of(context).platform`
  - `TargetPlatform` 열거형을 반환한다. iOS, Android, Fuschia 케이스를 갖는다.
- `Platform` 클래스
  - `isWindow`, `isAndroid`, `isIOS`, `isFuschia`, `isMacOS`, `isLinux` 프로퍼티를 통해 현재 앱이 실행 중인 플랫폼을 확인할 수 있다.

## Using Platform Specific Widgets

플랫폼을 확인하여 Android일 때 Material 패키지에 포함된 위젯을 나타내고 iOS일 때 Cupertino 패키지에 포함된 위젯을 나타내게 할 수 있다.

## Adding a Platform Specific Theme

MaterialApp 객체의 `theme` 프로퍼티에 ThemeData를 넘겨줄 수 있으며, 플랫폼별로 다른 ThemeData를 정의하여 넘겨줄 수 있다.

다른 위젯에서 Theme 객체를 통해 ThemeData에 접근하여 플랫폼별로 다르게 정의된 ThemeData를 사용할 수 있다.

이외에 플랫폼별로 분기하여 위젯의 프로퍼티에 각각 다른 값을 넘겨주어 플랫폼별로 다른 UI를 나타내게 할 수 있다.

## When Should we Use Platform Specific Themes?

기본적으로 Flutter를 사용하여 안드로이드와 iOS 각각의 사용자 경험을 유지하는 앱을 만드는 것은 Flutter가 추구하는 것이 아니다. 기존에 구현되어 있는 위젯을 활용하고 약간의 변화를 주어 요구 사항에 맞게 구현하는 것이 중요하다.

플랫폼별로 다른 UI를 구현해야 하는 요구 사항이 있을 때 두 디자인 시스템을 혼용하여 사용할 수 있겠다.

## iOS Support since Flutter 0.8.2

CupertinoApp, CupertinoPageScaffold, CupertinoRoute와 같은 위젯이 제공되어 iOS의 사용자 경험을 갖는 앱을 만들기 쉽게 되었다.

하지만 여전히 Flutter는 일반적으로 Material Design System을 활용하여 앱을 만드는 것을 권장한다.

## Wrap Up

- 앱이 실행 중인 플랫폼 확인하기
  - `Theme.of(context).platform` 과 `TargetPlatform` 열거형 비교하기
- Cupertino 위젯을 사용하여 iOS 스타일의 UI 요소 사용하기
- 결국에는 커스터마이징된 앱을 만들게 될 것이므로 안드로이드에 특화된 또는, iOS에 특화된 앱을 만드려고 하지 않도록 한다.
- 일반적으로 Material Design System을 구현한 위젯을 사용하고 필요하다면 그것을 커스터마이징한다.