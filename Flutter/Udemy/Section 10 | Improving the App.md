# Improving the App

## Module Introduction

애플리케이션의 사용성 개선 및 유지보수성 향상

## Improving the List Tile

CircleAvatar를 사용하면 이미지를 쉽게 원형으로 표현할 수 있다.

Creates a circle that represents a user. 사용자를 나타내는 원을 생성한다.

- `backgroundImage` 프로퍼티는 ImageProvider를 요구하는데, 그것이 하위 클래스인 AssetImage를 사용하여 에셋 이미지를 사용할 수 있다.
  - 이를 ListTile의 `leading` 프로퍼티에 넘겨주어 이미지를 좀더 보기 좋게 나타낼 수 있다.

ListTile의 `subtitle` 프로퍼티에 부제를 설정할 수 있다.

- iOS의 `UITableViewCell` 이 subtitle 스타일일 때와 비교 가능하다.

Divider 위젯을 사용하여 ListView에 구분선을 표현할 수 있다. ListView 만으로는 구분선을 표현할 수 없다.

- ListTile을 Column으로 감싸고 ListTile 다음에 Divider 위젯을 위치시킨다.

## Adding the Dismissible Widget

Swipe to Delete를 구현하려면 Dismissible 위젯에 ListTile을 감싸야 한다.

- `child` 프로퍼티에 dismissible하게 만들어줄 위젯을 넘겨준다.
- `key` 프로퍼티에 식별 가능한 유일한 값을 넘겨준다.
  - 모델의 identifier 등을 넘겨줄 수 있을 것이다.
  - dismiss하는 것은 UI에서 없애는 역할을 할 뿐이며 데이터를 없애지는 않는다. 이 key를 사용하여 해당 위젯이 다른 위젯들과 유일하게 식별될 수 있도록 한다.
- `background` 프로퍼티에 배경과 관련된 위젯을 넘겨준다.
  - 스와이프시 기본적으로 아무것도 표시되지 않으므로 이를 활용하여 시각적으로 나타낼 수 있게 한다.

## Deleting Products Upon Swipe

Dismissible 위젯

- `onDismissed` 프로퍼티에 스와이프하여 사라졌을 때 수행할 콜백 함수를 등록한다.
  - 스와이프 방향 (DismissDirection) 이 인자로 넘어온다.
    - `DismissDirection.endToStart` 는 읽기 순서의 반대 방향을 뜻한다. (일반적으로 오른쪽에서 왼쪽)
    - 개발자가 의도한 방향으로 사용자가 스와이프했을 때 삭제가 이루어지도록 할 수 있다.

## Restructing the Code & Wrap Up

수시로 리팩토링하기.

- 너무 깊게 들여쓰기된 하나의 메소드나 위젯의 구현을 메소드나 클래스로 분리하기