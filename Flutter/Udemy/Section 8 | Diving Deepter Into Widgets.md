# Diving Deeper Into Widgets

## Module Introduction

위젯 카탈로그 참고하기

위젯을 잘 조합하여 좋은 UI 만들어내기

## Exploring the Widget Catalogue

- Basic Widgets
  - Container, Row, Column, Text 등
  - UI를 만드는 데 있어서 알아야 할 기본 위젯들

공식 문서를 가까이 하기

## There's More Than One Widget For The Job

어떠한 작업을 하기 위한 여러 가지 방법이 존재할 수 있다.

- 여백 나타내기
  - SizedBox 위젯 활용
  - Container로 감싸 margin이나 padding 주기
    - margin은 offset, padding은 inset 느낌. margin은 바깥으로, padding은 안으로.
    - `EdgeInsets.all` : 지정한 수치만큼 상하좌우 모두에 적용
    - `EdgeInsets.only` : top, bottom 등 각각에 수치 적용
    - `EdgeInsets.symmetric` : vertical는 상하, horizontal은 좌우에 수치 적용
  - Padding 위젯 또는 Margin 위젯으로 감싸기

## Working with Text & Fonts

Text 위젯의 `style` 프로퍼티에 TextStyle 오브젝트를 넘겨 주어 스타일 설정

폰트 파일을 프로젝트에 추가하고 `pubspec.yaml` 파일에 명시하기

```yaml
fonts:
  - family: Oswald
    fonts:
      - asset: assets/Oswald-Bold.ttf
        weight: 700
```

weight: 700은 볼드체의 기본 두께

이탤릭체인 경우 style: italic을 추가로 명시할 필요가 있음

```yaml
- asset: fonts/Schyler-Italic.ttf
  style: italic
```

TextStyle에 fontFamily를 명시하여 각각의 Text가 특정 폰트를 사용하게끔 할 수 있다.

MaterialApp의 `fontFamily` 프로퍼티에 폰트 패밀리를 명시하여 앱 전역적으로 특정 폰트를 사용하게끔 할 수도 있다.

## Working With Rows

Row의 경우 MainAxis는 수평, CrossAxis는 수직

Column의 경우 MainAxis는 수직, CrossAxis는 수평

MainAxisAlignment의 spaceXXX 케이스를 사용하여 iOS의 `UIStackView` 의 기능을 사용할 수 있음 (요소 간 동일 간격 등)

## Decorating Boxes & Styling a Price Tag

DecoratedBox 위젯에 감싸 해당 컨테이너에 대한 데코레이션(border / border radius / drop shadow 등)을 쉽게 할 수 있다.

위의 행위는 Container 위젯에 감싸도 동일하게 구현할 수 있다.

Text 위젯을 Container로 감싸 Text에 대한 스타일링, Text를 감싸는 컨테이너에 대한 스타일링을 각각 구현해야 한다.

---

### Dart

문자열 보간법

- `$변수명` 또는 `${표현식}`

Escaping Character

- `\$`

---

## Setting Borders

BoxDecoration 클래스의 `border` 프로퍼티에는 일반적으로 Border 오브젝트를 넘겨준다.

SDK 소스 코드를 분석하여 어떠한 프로퍼티를 가지고 있고 어떻게 구현되었는지 살피면서 프로그래밍하는 것이 좋다.

## Understanding "Expanded" & "Flexible"

- Expanded
  - 해당 위젯이 차지할 수 있는 최대한의 공간을 차지함 (flex 프로퍼티 조절)
- Flexible
  - 해당 위젯은 유연하게 공간 점유 정도를 결정할 수 있음 (fit, flex 프로퍼티 조절)
    - `fit` : FlexFit 열거형을 넘겨줌. loose / tight
- `flex` : int. 남은 공간에서 해당 위젯의 child 위젯이 flex개 들어갈 수 있도록 크기 조정

Expanded는 모든 사용 가능한 공간을 차지한다.

Flexible은 모든 사용 가능한 공간을 차지할 수 있으나 fit 인자로 조절할 수 있다.

`flex` 는 상대적인 개념. 비율적으로 공간 분배

여러 실험을 통해 이해해야 할듯. 한정된 공간 내에서 균일한 요소 분배의 필요가 있을 때 활용 가능하다.

## Adding a Background Image

`decoration` 프로퍼티에 넘겨줄 수 있는 BoxDecoration 클래스의 `image` 프로퍼티에 DecorationImage 오브젝트를 넘겨줄 수 있다.

이것의 `image` 에 AssetImage 오브젝트를 넘겨줄 수 있다.

BoxFit 열거형의 케이스

- `cover` : 원본을 왜곡하지 않고 채움
- `fill` : 원본을 왜곡하고 채움

DecorationImage 클래스의 `colorFilter` 프로퍼티를 통해 필터를 적용할 수 있음

- `ColorFilter.mode`
- `BlendMode.dstATop` : 이미지에 투명도가 있는 색상을 올려 필터링

## Centering Input Fields

ListView는 자동으로 차지할 수 있는 최대한의 높이를 가지므로 이를 Center 위젯으로 감싼다고 해서 중앙 정렬되지 않는다.

SingleChildScrollView : 하나의 child를 갖는 스크롤 가능한 뷰.

ListView 대신 SingleChildScrollView를 사용하면 리스트의 형태가 아니지만 스크롤의 행위를 원하는 컨테이너를 스크롤 가능하게 할 수 있다.

## Adding Icons to our Sidemenu

ListTile의 `leading` 프로퍼티에 아이콘을 넘겨줄 수 있다.

다른 위젯도 들어갈 수 있으나 Icon이나 CircleAvatar를 넘겨주는 것이 권장된다.

## Adding Icon Buttons

IconButton 위젯을 사용하여 텍스트 대신 아이콘의 형태로 나타나는 버튼을 만들 수 있다.

iOS의 이미지를 갖는 `UIBarButtonItem`이나 `UIButton` 과 비교 가능하다.

AppBar의 `actions` 프로퍼티에 IconButton을 넘겨줄 수 있다.

## Outsourcing Code into Separate Widgets

깊게 중첩된 UI를 분리하여 가독성과 재사용성 높이기

판단은 개발자의 몫.

## Refactoring our Code

Page와 Widget을 구분하여 위젯을 추출하여 정리하기

- Page는 iOS의 `UIViewController` 처럼 전체 페이지를 관장하는 역할
- Widget은 iOS의 `UIView` 처럼 어떠한 커스텀 뷰를 나타냄

## Creating a Standardized Tile Widget

위와 같은 이야기. 일정한 패턴을 가지고 중복된 코드 형태를 갖는 UI 코드를 분리하여 정리하면 가독성 및 재사용성을 높일 수 있다.

## Adding Separate Methods for Specific Widgets

`build` 메소드를 길게 작성하는 것을 권장하지 않는다.

이를 분리시켜 `build` 메소드의 구현부를 짧게 유지하자.

특정 위젯이 재사용되기보다는 파일 내에서 긴 코드에 의해 작성되었다면 이를 메소드로 분리하는 것도 좋은 방법이다.

## Optimizing our Project

위에서 배운 테크닉을 활용하여 프로젝트 리팩토링하기.

## Responsive Design Problems

기기의 방향, 다양한 크기에 대응하기

Center 위젯에 감싸는 대신 Container 위젯에 감싸 `alignment` 프로퍼티를 center로 설정해주면 같은 효과를 얻을 수 있음과 동시에 `width` 와 `height` 도 각각 설정해줄 수 있다.

하지만 width와 height를 고정값으로 설정해주는 것은 좋지 않다.

## Adding Media Queries

MediaQuery 클래스 사용하여 현재 컨텍스트에 대한 디바이스 데이터를 얻을 수 있다.

- 화면 크기 (`size`)
- 기기 방향 (`orientation`)
- 등등

예를 들어 Container의 `width` 에 `MediaQuery.of(context).size.width * 0.8` 을 넘겨주어 현재 기기의 너비의 80%만큼을 컨테이너의 너비 값으로 줄 수 있으며, 때문에 화면 방향에 상관 없이 일관된 UX를 제공할 수 있다.

MediaQuery가 제공하는 값을 조작하여 세로 화면과 가로 화면에 적절한 UI를 그리게 할 수 있다.

iOS의 `UIDevice` 나 `UIScreen ` 등과 비교 가능하다.

## Understanding Media Queries with ListView

ListView는 수직 방향 ListView의 경우 차지할 수 있는 모든 너비를 차지한다.

그러므로 컨테이너에 width를 주는 것이 아닌 padding을 주어 가로 및 세로 방향에 대응하도록 한다.

## Working with Themes

ThemeData 클래스에 있는 프로퍼티 다루기.

- `brightness` : 밝은 테마 또는 어두운 테마
- `primarySwatch` : 앱의 주 색상
- `accentColor` : 앱의 강조 색상

이처럼 테마를 미리 지정해 두고 다르게 설정해야 하는 부분에만 따로 프로퍼티에 값을 할당해주는 식으로 하는 것이 좋을 것이다.

iOS에서 각 UI 관련 클래스의 `appearance`와 비교 가능하다.

## Listening to Touch Events with the Gesture Detector

위젯을 GestureDetector 클래스로 감싸 제스처를 달 수 있다.

- `onTap`, `onLongPress`, `onHorizontalDragCancel` 등 다양한 제스처에 대한 콜백 함수를 등록할 수 있다.

iOS의 `UIGestureRecognizer` 와 비교 가능하다.

## Wrap Up

- Flutter는 Widget의 모음이므로 Widget에 친숙해지자.
  - Widget Catalogue 참고하기
  - 하려는 작업에 적합한 위젯을 사용할 수 있게 하기
- Container / Row / Column / ListView / FlatButton / RaisedButton / SizedBox / TextField / Card / Scaffold / Icon / Image
  - 자주 사용하는 위젯은 한정되어 있다. 이런 것들에 먼저 익숙해지고 필요할 때마다 위젯 카탈로그를 참고할 수 있도록 하자.
- 어떠한 작업을 하기 위해 사용할 수 있는 위젯의 조합은 여러 개이므로 실험을 통해 최적의 조합을 찾아내자.
- 리팩토링의 생활화
  - `build` 메소드 구현을 짧게 하기 위해 위젯을 메소드로 분리하기
  - 재사용되는 위젯은 클래스로 분리하기
- 반응형 디자인
  - MediaQuery 클래스 사용하여 디바이스의 크기와 방향 구하기
  - Row, Column, ListView와 같은 유연한 위젯 사용하기