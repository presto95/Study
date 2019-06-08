# Navigation

## Module Introduction

화면 간 내비게이션 (페이지 변경)

모바일 애플리케이션에서 한 화면에서 모든 일이 이루어지기는 힘들다.

## Adding Multiple Pages to Our App

Scaffold 위젯을 하나의 화면으로 생각해도 좋다.

## Adding Buttons

ButtonBar 위젯을 사용하여 여러 개의 버튼을 한 곳에 몰아넣는 컨테이너의 역할을 하도록 할 수 있다.

## Implementing a Basic Navigation

**Navigator 클래스 사용**

Flutter에서 내비게이션은 스택으로 이루어지므로 push와 pop의 개념을 사용한다.

```dart
Navigator.push(
  context, 
  MaterialPageRoute(
    builder: (context) => ProductPage(),
  ),
);
//////
Navigator.of(context).push(
  MaterialPageRoute(
    builder: (context) => ProductPage(),
  ),
)
```

Navigator에 Route를 제공하여 내비게이션을 수행한다.

이를 위해 현재 컨텍스트가 필요하다.

```dart
Navigator.pop(context);
//////
Navigator.of(context).pop();
```

pop을 통하여 이전 화면으로 돌아간다.

## Improving the Look of the Product Page

Column의 경우 mainAxis(주축)은 수직, crossAxis(보조축)은 수평 방향이다.

Row의 경우 mainAxis는 수평, crossAxis는 수직 방향이다.

MainAxisAlignment 클래스에서 시작 / 중간 / 끝에 정렬할 수 있고, 동일한 계층에 있는 위젯 간 균일한 간격을 갖도록 설정할 수 있다. (start / center / end / spaceBetween / spaceAround / spaceEvenly)

CrossAxisAlignment 클래스에서 시작 / 중간 / 끝에 정렬할 수 있고, 빈 공간을 채우도록 할 수 있다. (start / center / end / baseline / stretch)

Column은 그 컨텐츠에 의해 크기가 결정된다.

Container 등에 감싸 위젯의 패딩 / 마진 등을 조절할 수 있다. Padding이나 Margin에 감쌀 수도 있다. UI 작성시 중요할 것.

## Passing Data Around("Pushing" the Page)

- 생성자를 통한 데이터 전달
  - 각각의 값을 전달하기
  - List나 Map과 같은 자료구조에 담아 전달하기
    - List는 제네릭 타입으로 요소의 타입을 명시한다.
    - Map은 제네릭 타입으로 키와 값의 타입을 명시한다.
      - `dynamic` 타입은 iOS에서 `Any` 와 비교 가능하다.

## Passing Information Back ("Popping" the Page)

`Navigator.push` 는 `Future<T>` 를 반환한다. Future는 단일 아이템에 대해 비동기적으로 반환되며, push된 객체가 pop될 때 일어난다.

그러므로 `Navigator.push` 에 메소드 체이닝으로 `then` 을 사용하여 pop될 때 넘겨받은 값을 사용하여 처리할 수 있다.

`then` 에 등록된 익명 함수는 Future가 완료될 때 호출된다.

pop될 때 `result` 프로퍼티에 값을 할당하여 값을 전달할 수 있다. `result` 는 Route의 결과를 나타낸다.

```dart
Navigator.push<bool>(...).then((value) => ...);

Navigator.pop(context, true);
```

iOS에서 completion handler를 통해 작업 완료 후 어떠한 작업을 수행하는 것과 비슷하다.

## Adding Reactions Upon Button Pressing

WillPopScope로 Scaffold 위젯을 감싸 pop될 때를 감지하여 어떠한 처리를 할 수 있다.

- 시스템의 기본 행위를 가로막았으므로 이전 화면으로 돌아가는 것 등의 처리를 해주어야 한다.
- `onWillPop` 프로퍼티는 `Future` 객체를 반환하는 익명 함수를 요구한다.

```dart
onWillPop: () {
  Navigator.of(context).pop(false);
  return Future.value(false);
}
```

기본적으로 Future가 가지고 있는 값이 true이면 pop되고 false이면 pop되지 않는다.

이전 화면으로 false 값을 보내기 위해 `Navigator.pop` 을 사용하는 것인데, 이를 사용하면 기본적으로 pop된다.

그러므로 Future가 true를 가지고 있다면 pop된 후 한번 더 pop을 시도하게 되며 에러를 발생시킨다.

pop의 행위를 `Navigator.pop` 이 하게 하고, Future는 false를 갖게 하여 해결할 수 있다.

## Adding the Basic Authentication Page & Replacing Routes

`Navigator.pushReplacement` 를 사용하여 현재 화면을 다른 화면으로 대체할 수 있다. push하여 내비게이션 스택에 쌓는 것이 아니라 현재 화면이 대체되는 것.

App Bar가 있는 경우 백 버튼이 자동 생성되지 않는다.

## Adding the Sidedrawer & the Hamburger Icon

- Material Widget을 활용하여 햄버거 버튼을 통한 Side Drawer 구현을 쉽게 할 수 있다. 이는 iOS가 기대하는 UX는 아니지만 편리할 것이다.
- Scaffold의 `drawer` 나 `endDrawer` 프로퍼티를 통해 Drawer를 구현할 수 있다. 기본적으로 drawer는 왼쪽, endDrawer는 오른쪽에 위치한다.
  - `drawer` 를 구현한 경우 상단의 App Bar 왼쪽에 햄버거 버튼이 자동 생성된다.
  - `endDrawer` 를 구현한 경우 오른쪽에 햄버거 버튼이 자동 생성된다.

AppBar 위젯은 Scaffold 위젯의 `appBar` 프로퍼티에 보통 사용되지만 다른 곳에서 사용해도 상관 없다. 이 경우 툴바의 역할을 하도록 구현할 수 있다.

ListTile 위젯은 ListView의 행을 구현할 때 사용 가능하다. 리스트 아이템을 표현하기 위한 내장 기능을 제공하는 편리한 위젯이다.

## Understanding Stack Based Navigation

`Navigator.pushReplacement` 를 통해 교체된 페이지는 메모리에서 해제된다. 내비게이션 스택에 쌓이는 것이 아니라 대체되는 것이기 때문이다.

탭과 Drawer는 각각의 내비게이션 스택을 전환할 수 있다.

- 하나의 탭이 하나의 내비게이션 스택을 갖는다.

## Adding Tabs

Material Design System에서 Tab은 App Bar의 하단에 위치하여 스와이프를 통해 컨텍스트를 전환할 수 있는 것을 말한다. iOS의 `UITabBarController` 와는 다르다.

DefaultTabController 위젯에 감싸 구현 가능하다.

- `length` 프로퍼티를 필수적으로 요구하며, 탭의 개수를 의미한다.
- AppBar의 `bottom` 프로퍼티에 TabBar 위젯을 넘겨주어야 한다.
  - TabBar의 `tabs` 프로퍼티에 각각의 탭에 대한 정보를 넘겨준다. 일반적으로 Tab 위젯을 사용한다.
- DefaultTabController에 감싸진 Scaffold의 `body` 프로퍼티에 TabBarView 위젯을 넘겨준다.
  - `children` 의 개수는 DefaultTabController의 `length` 와 같아야 한다.

## Finishing the Tab Navigation

TabBarView의 `children` 에 들어갈 위젯은 페이지의 body를 구현한 것이어야 한다. 그것은 하나의 화면이라기보다는 탭 컨트롤러에 의해 전환되는 body이다.

### Tab UI 구현하기

- 탭 인터페이스를 사용할 화면(Scaffold)을 DefaultTabController로 감싼다.
  - `length` 를 지정한다. 이는 TabBarView의 하위 위젯의 개수와 같아야 한다.
- Scaffold 위젯의 App Bar의 `bottom` 에 TabBar를 넘겨준다.
  - 탭의 개수만큼 Tab을 추가한다.
- Scaffold 위젯의 `body` 에 TabBarView를 넘겨준다.
  - TabBarView의 `children` 에 각 Tab에 대한 뷰가 위치하게 된다.
  - 한 화면이 아닌 탭에 따라 보여지는 뷰일 뿐이므로 Scaffold에 감싸지 않도록 한다.

## Adding Named Routes

MaterialApp의 `routes` 에 Route를 등록하여 Route에 이름(String)을 부여할 수 있고, 이후 내비게이션 작업에 이를 활용할 수 있다.

`route` 에는 `Map<String, WidgetBuilder>` 타입이 들어가고, `WidgetBuilder` 는 `BuildContext -> Widget` 의 타입 별칭이다.

`Navigator.pushNamed` 나 `Navigator.pushReplacementNamed` 를 활용할 수 있다.

/ Route는 특별한 것이며 첫 화면을 나타낸다. 이를 명시하면 `home` 프로퍼티에 값을 넘겨주면 안된다.

```dart
MaterialApp(
  routes: {
    "/": (context) => HomePage(),
    "/management": (context) => ProductManagementPage(),
  }
);

Navigator.of(context).pushNamed("/management");
```

Route에 이름을 주어 내비게이션하는 것은 편리하지만 생성자를 통한 데이터 전달이 필요할 때 사용하기 힘들다.

## Parsing Route Data Manually

MaterialApp의 `onGenerateRoute` 프로퍼티에 Route를 생성하는 로직을 추가한다.

이는 `routes` 에 등록되지 않은 Route여야 수행된다.

## Lifting State Up

하위 계층에서 관리하던 상태를 상위 계층으로 올려 관리하기.

상태를 가진다 == Stateful Widget

## Using the Named Route

Route의 이름을 지을 때는 웹 개발시 각 엔드포인트를 정의하는 것처럼 하는 것이 좋다.

`routes` 에는 정적으로 내비게이션되는 화면을 정의한다.

`onGenerateRoute` 에는 넘어온 Route Setting을 처리하여 동적으로 내비게이션되는 화면을 정의한다. 웹 개발시 URL에 query string이나 semantic query가 붙는 경우를 생각하면 될 것.

## Working with "onUnknownRoute" as Fallback

MaterialApp의 `onUnknownRoute` 에 등록된 익명 함수는 `onGenerateRoute` 가 Route 생성을 실패했을 때 수행된다.

`routes` -> `onGenerateRoute` -> `onUnknownRoute`

네트워크 요청에 의한 내비게이션이 필요할 때 해당 콜백 함수가 실행될 여지가 있다. 404 페이지 등으로 내비게이션하도록 처리할 수 있을 것이다.

---

### VSCode

F2를 눌러 프로젝트 내의 전체 파일에 대한 변수명 또는 함수명의 리팩토링을 진행할 수 있다.

---

## Adding Alert Dialogs

모달로 뜨는 Alert Dialog 또한 Navigator가 관리하므로 `Navigator.pop` 을 통해 dialog를 닫는다.

`showDialog` 함수를 사용하여 dialog를 띄울 수 있으며 Material 패키지는 AlertDialog 위젯을 제공하여 쉽게 dialog를 만들 수 있다.

```dart
showDialog(
  context: context,
  builder: (context) {
    return AlertDialog(
      title: Text("Title"),
      content: Text("Content"),
      actions: [
        FlatButton(
          child: Text("OK"),
          onPressed: () => Navigator.of(context).pop(),
        ),
        FlatButton(
          child: Text("Cancel"),
          onPressed: () => Navigator.of(context).pop(),
        ),
      ],
    );
  }
);
```

`actions`에는 일반적으로 FlatButton 위젯이 들어간다.

Future를 반환하므로 `then` 메소드를 사용하여 작업 완료 후 처리를 할 수 있다.

## Showing a Modal

`showModalBottomSheet` 함수를 사용하여 하단에 모달로 뷰를 나타나게 할 수 있다.

`showBottomSheet` 함수는 하단에서 슬라이드로 올라오는 뷰를 나타내며, 모달로 뜨지는 않는다.

`showModalBottomSheet` 와 `showDialog` 의 행위는 iOS의 `UIAlertController` 의 `actionSheet` 와 `alert` 스타일과 비교하면 될 것 같다.

`showModalBottomSheet` 함수를 사용하여 나타난 뷰도 Navigator가 관리한다.

## Wrap Up

- Navigator는 직접적으로 위젯을 push하지 않고, MaterialPageRoute와 같은 Route를 사용한다.
- 생성자를 통한 데이터 전달
- `Navigator.pop` 으로 데이터를 이전 화면에 돌려줄 수 있고, `Navigator.push` 가 반환한 Future 객체를 사용하여 (`then` 메소드 사용) 처리할 수 있다.
- App에 이름 있는 Route를 등록하여 내비게이션 처리를 할 수 있다.
  - `routes` : 정적 내비게이션
  - `onGenerateRoute` : Route 파싱을 통한 동적 내비게이션
  - `onUnknownRoute` : 404 페이지
- Dialog나 Modal도 Navigator가 관리한다.
  - Dialog나 Modal에서 발생한 pop은 그 자체를 사라지게 할 뿐이다.