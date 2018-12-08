[Introduction to declarative UI](https://flutter.io/docs/get-started/flutter-for/declarative)

# 선언형 UI 소개

이 소개는 Flutter에서 사용되는 선언형 스타일과 많은 다른 UI 프레임워크에서 사용되는 명령형 스타일 간 개념적 차이를 서술합니다.

## 왜 선언형 UI입니까?

Win32부터 웹과 안드로이드 및 iOS의 프레임워크는 UI 프로그래밍에 선언형 스타일을 사용합니다. 이것은 당신에게 가장 친숙한 스타일일 것입니다. 수작업으로 완전한 기능을 하는 `UIView`나 이와 동등한 UI 엔터티를 구축하고, UI가 변화할 때 메소드나 설정자를 사용하여 나중에 그것을 변화시킵니다.

개발자가 다양한 UI 상태 간에 전이하는 방법을 프로그래밍하는 것에 대한 부담을 줄이기 위해, 대조적으로 Flutter는 개발자가 현재 UI 상태를 서술하고 프레임워크에 그 전이를 맡기도록 하였습니다.

그러나 이것은 UI를 다루는 방법에 대해 약간의 생각의 전환을 필요로 합니다.

## 어떻게 선언형 프레임워크에서 UI를 변경합니까?

아래에 있는 간단한 예시를 고려합시다.

![View B (contained by view A) morphs from containing two views, c1 and c2, to containing only view c3](https://flutter.io/images/declarativeUIchanges.png)

명령형 스타일에서는 일반적으로 `ViewB`의 소유자로 가서 셀렉터나 `findViewById`, 또는 이와 비슷한 방법으로 `b` 인스턴스를 가져올 것입니다. 그리고 그것에 변화를 일으킵니다(암시적으로 무효화합니다). 예를 들자면 아래와 같습니다.

```
// 명령형 스타일
b.setColor(red)
b.clearChildren()
ViewC c3 = new ViewC(...)
b.add(c3)
```

UI에 대한 진실의 소스가 `b` 인스턴스 자체보다 더 오래 남아 있기 때문에 `ViewB`의 생성자에서 이 구성을 복제할 필요도 있을 수 있습니다.

선언형 스타일에서 Flutter의 위젯과 같은 뷰 구성은 불변하며 가벼운 "청사진"일 뿐입니다. UI를 변경하기 위해 위젯은 그 자체에서 다시 만들어지는 것을 촉발하며 새로운 위젯 서브트리를 구축합니다. Flutter의 `StatefulWidget`에서는 `setState()` 메소드를 호출하여 흔하게 사용됩니다.

```
// 선언형 스타일
return ViewB(
  color: red,
  child: ViewC(...),
)
```

UI가 변화할 때 오래된 `b` 인스턴스를 변화시키는 대신, Flutter는 새로운 위젯 인스턴스를 생성합니다. 프레임워크는 레이아웃의 상태를 관리하는 것과 같은 전통적인 UI 오브젝트에 책임이 있는 많은 것들을 `RenderObject`에서 우리가 모르게 관리합니다. `RenderObject`는 프레임 간에 지속되며, Flutter의 가벼운 위젯은 프레임워크에게 상태 간에 `RenderObject`를 변화시키라고 알립니다. Flutter 프레임워크는 나머지 것들을 처리합니다.