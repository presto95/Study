# iOS 개발자를 위한 Flutter

이 문서는 Flutter로 모바일 앱을 만드는데 이미 가지고 있는 iOS에 대한 지식을 적용하려고 하는 iOS 개발자를 위한 것입니다. iOS 프레임워크의 근본을 이해한다면 이 문서를 Flutter 개발을 배우는 것을 시작하는 수단으로 사용할 수 있습니다.

당신의 iOS 지식과 스킬 셋은 Flutter로 앱을 만들 때 매우 가치있습니다. Flutter가 수많은 능력과 구성을 위한 모바일 운영체제에 의존하기 때문입니다. Flutter는 모바일에서 UI를 만드는 새로운 방법이지만, UI가 아닌 작업에 대해 iOS 및 안드로이드와 소통하기 위한 플러그인 시스템을 가지고 있습니다. 당신이 iOS 개발의 전문가라면 Flutter를 사용하기 위해 모든 것을 다시 배울 필요는 없습니다.

이 문서는 쿡북으로 사용될 수 있으며 당신의 니즈와 가장 관련 있는 질문을 찾을 때도 사용될 수 있습니다.

## 뷰*Views*

### Flutter에서 `UIView`와 동등한 것은 무엇입니까?

> 리액트 스타일(선언형) 프로그래밍은 전통적인 명령형 스타일과 어떻게 다릅니까? 비교를 위해 [선언형 UI 입문](https://flutter.io/docs/get-started/flutter-for/declarative)을 참고하십시오.

iOS의 UI에서 당신이 만드는 대부분의 것은 `UIView` 클래스의 인스턴스인 뷰 오브젝트를 사용하여 이루어집니다. 이러한 것들은 다른 `UIView` 클래스들을 위한 컨테이너로 동작하여 레이아웃을 형성합니다.

Flutter에서 `UIView`와 비슷한 것은 `Widget`입니다. 위젯은 iOS의 뷰와 정확하게 매핑되지는 않으나, Flutter가 동작하는 방식에 익숙해지는 동안에는 위젯을 "UI를 선언하고 조립하는 방법"으로 생각하는 것이 좋습니다.

그러나 위젯은 `UIView`와 몇 가지 차이점을 가지고 있습니다. 일단 위젯은 다른 수명을 가지고 있습니다. 위젯은 불변하며 변화할 필요가 있을 때까지만 존재합니다. 위젯이나 그 상태가 변화할 때마다 Flutter의 프레임워크는 새로운 위젯 인스턴스 트리를 생성합니다. 비교해보면 iOS의 뷰는 변화할 때 다시 생성되지 않습니다. 오히려 한번 그려지면 `setNeedsDisplay()`를 사용하여 그것을 무효로 만들 때까지 다시 그리지 않는 가변 엔티티입니다.

게다가 `UIView`와는 다르게 Flutter의 위젯은 그 불변성으로 인해 가볍습니다. 위젯은 그 자체로 뷰가 아니며 어떠한 것도 직접적으로 그리지 않기 때문에, 프로그램 아래에서 실제 뷰 오브젝트 내로 팽창되는*inflated* UI 및 그 의미에 대한 묘사일 뿐입니다.

Flutter는 [머터리얼 컴포넌트](https://material.io/develop/flutter/) 라이브러리를 포함하며, [머터리얼 디자인 가이드라인](https://material.io/design/)을 구현하는 위젯을 포함합니다. 머터리얼 디자인은 iOS를 포함하여 [모든 플랫폼에 최적화된](https://material.io/design/platform-guidance/cross-platform-adaptation.html#cross-platform-guidelines) 유연한 디자인 시스템입니다.

하지만 Flutter는 어떠한 디자인 언어라도 구현하기에 충분히 유연하고 표현적입니다. iOS에서는 [애플의 iOS 디자인 언어](https://developer.apple.com/design/resources/)처럼 보이는 인터페이스를 만들기 위해 [쿠퍼티노 위젯](https://flutter.io/docs/development/ui/widgets/cupertino)을 사용할 수 있습니다.

### 어떻게 `Widget`을 업데이트합니까?

iOS에서는 뷰를 업데이트하기 위해 직접적으로 그것을 변화시켰습니다. Flutter에서 위젯은 불변하며 직접적으로 업데이트되지 않습니다. 대신 위젯의 상태를 다뤄야 합니다.

이것이 상태 있는 위젯 및 상태 없는 위젯의 개념이 비롯되는 곳입니다. `StatelessWidget`은 그 뜻대로, 어떠한 상태도 붙어있지 않은 위젯을 말합니다.

`StatelessWidget`은 묘사하는 UI의 일부분이 위젯의 초기 구성 정보 이외에 어떠한 것에도 의존하지 않을 때 유용합니다.

예를 들어 iOS에서 이는 `UIImageView`를 두어 로고를 `image`로 나타내는 것과 비슷합니다. 로고가 런타임 동안 변화하지 않는다면, Flutter에서 `StatelessWidget`을 사용하십시오.

HTTP 요청 이후 받은 데이터에 의존하여 UI를 동적으로 변화시키기 원한다면 `StatefulWidget`을 사용하십시오. HTTP 요청이 완료된 후 Flutter 프레임워크에게 위젯의 `State`가 업데이트되었다고 말하여 UI를 업데이트할 수 있게 하십시오.

상태 없는 위젯과 상태 있는 위젯의 중요한 차이점은, `StatefulWidget`은 상태 데이터를 저장하고 트리 재구성을 통해 전달되는 `State` 오브젝트를 가지고 있기 때문에 상태가 손실되지 않는다는 것입니다.

의심스럽다면, 다음의 규칙을 기억하십시오. 위젯이 런타임에서의 사용자 상호작용 등의 이유로 `build` 메소드 바깥에서 변화한다면, 그것은 상태가 있는 것입니다. 위젯이 절대 변화하지 않는다면, 그것은 상태가 없는 것입니다. 하지만 위젯이 상태가 있는 것일지라도, 그것을 포함하는 부모 위젯은 그 자체가 저러한 변화나 다른 입력에 반응하지 않는다면 여전히 상태가 없는 것일 수 있습니다.

다음의 예시는 `StatelessWidget`을 사용하는 방법을 보여줍니다. `Text` 위젯은 `StatelessWidget`의 흔한 예시입니다. `Text` 위젯의 구현을 확인한다면 그것이 `StatelessWidget`의 서브클래스임을 알 수 있을 것입니다.

```dart
Text(
    'I like Flutter!',
    style: TextStyle(fontWeight: FontWeight.bold),
);
```

위에 있는 코드에서 `Text` 위젯이 그것 내부에 어떤 명백한 상태를 전달하지 않는다는 것을 확인할 수 있을 것입니다. 그것은 그 생성자에서 넘겨진 것을 렌더링할 뿐 더 많은 일을 하지 않습니다.

하지만 예를 들어 `FloatingActionButton`을 클릭할 때 "I like Flutter!"를 동적으로 변화하게 하고 싶다면 어떻게 해야 합니까?

이를 달성하기 위해 `Text` 위젯을 `StatefulWidget` 내부에 감싸고 사용자가 버튼을 클릭할 때 그것을 업데이트 하십시오.

예를 들어 아래와 같이 코드를 작성할 수 있습니다.

```dart
class SampleApp extends StatelessWidget {
  // 이 위젯은 애플리케이션의 루트입니다.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // 기본 플레이스홀더 텍스트
  String textToShow = "I Like Flutter";
  void _updateText() {
    setState(() {
      // 텍스트 업데이트
      textToShow = "Flutter is Awesome!";
    });
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(child: Text(textToShow)),
      floatingActionButton: FloatingActionButton(
        onPressed: _updateText,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}
```

### 어떻게 위젯을 배치합니까? 스토리보드는 어디에 있습니까?

iOS에서 뷰를 조직하고 제약 조건을 설정하기 위해 스토리보드를 사용했을 것이며, 뷰 컨트롤러 내에서 코드로 제약 조건을 설정해주었을 것입니다. Flutter에서는 위젯 트리를 구성하여 코드로 레이아웃을 선언합니다.

다음의 예시는 패딩이 추가된 간단한 위젯을 표시하는 방법을 보여줍니다.

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: CupertinoButton(
        onPressed: () {
          setState(() { _pressedCount += 1; });
        },
        child: Text('Hello'),
        padding: EdgeInsets.only(left: 10.0, right: 10.0),
      ),
    ),
  );
}
```

어떠한 위젯이라도 패딩을 추가할 수 있으며, iOS에서의 제약 조건의 기능을 흉내낼 수 있습니다.

[위젯 목록](https://flutter.io/docs/development/ui/widgets/layout)에서 Flutter가 제공하는 레이아웃을 확인할 수 있습니다.

### 어떻게 레이아웃에서 컴포넌트를 추가하거나 삭제합니까?

iOS에서는 동적으로 자식 뷰를 추가하거나 삭제하기 위해 부모 뷰에서 `addSubview()`를, 자식 뷰에서 `removeFromSuperview()`를 호출합니다. Flutter에서 위젯은 불변하므로 `addSubview()`와 직접적으로 동등한 것은 없습니다. 대신 위젯을 반환하는 부모에게 함수를 넘겨줄 수 있으며, 불리언 플래그로 자식의 생성을 관리할 수 있습니다.

다음의 예시는 사용자가 `FloatingActionButton`을 클릭할 때 두 위젯 간에 토글하는 방법을 나타냅니다.

```dart
class SampleApp extends StatelessWidget {
  // 이 위젯은 애플리케이션의 루트입니다.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // 토글을 위한 기본값
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  _getToggleChild() {
    if (toggle) {
      return Text('Toggle One');
    } else {
      return CupertinoButton(
        onPressed: () {},
        child: Text('Toggle Two'),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: _getToggleChild(),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _toggle,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}

```

### 어떻게 위젯에 애니메이션 효과를 줄 수 있습니까?

iOS에서는 뷰에 `animate(withDuration:animations:)` 메소드를 호출하여 애니메이션을 생성합니다. Flutter에서는 위젯을 애니메이션 위젯 내부에 감싸기 위해 애니메이션 라이브러리를 사용합니다.

Flutter에서는 `AnimationController`를 사용하는데, 이것은 애니메이션을 일시정지, 탐색, 중지 및 순서를 거꾸로 하기 위한 `Animation<double>`입니다. 수직 동기화가 발생했음을 알리고 그것이 동작하는 동안 각 프레임에 0과 1 사이의 선형 보간을 생산하는 `Ticker`를 필요로 합니다. 그리고 나서 하나 또는 그 이상의 `Animation`을 생성하고 컨트롤러에 부착합니다.

예를 들어 `CurvedAnimation`을 사용하여 보간된 곡선을 따르는 애니메이션을 구현할 수 있을 것입니다. 이러한 의미에서 컨트롤러는 애니메이션 진행의 '마스터' 소스이며 `CurvedAnimation`은 컨트롤러의 기본 선형 모션을 교체하는 곡선을 계산합니다. Flutter의 애니메이션은 위젯처럼 조립을 통해 동작합니다.

위젯 트리를 만들 때 `FadeTransition`의 불투명도와 같은 위젯의 애니메이션 가능한 프로퍼티에 `Animation`을 할당하고 컨트롤러에게 애니메이션을 시작하라고 말합니다.

다음의 예시는 `FloatingActionButton`을 누를 때 로고 안에 있는 위젯이 서서히 사라져 가는 `FadeTransition`을 작성하는 방법을 나타냅니다.

```dart
class SampleApp extends StatelessWidget {
  // 이 위젯은 애플리케이션의 루트입니다.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fade Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyFadeTest(title: 'Fade Demo'),
    );
  }
}

class MyFadeTest extends StatefulWidget {
  MyFadeTest({Key key, this.title}) : super(key: key);

  final String title;

  @override
  _MyFadeTest createState() => _MyFadeTest();
}

class _MyFadeTest extends State<MyFadeTest> with TickerProviderStateMixin {
  AnimationController controller;
  CurvedAnimation curve;

  @override
  void initState() {
    controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
    curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Container(
          child: FadeTransition(
            opacity: curve,
            child: FlutterLogo(
              size: 100.0,
            )
          )
        )
      ),
      floatingActionButton: FloatingActionButton(
        tooltip: 'Fade',
        child: Icon(Icons.brush),
        onPressed: () {
          controller.forward();
        },
      ),
    );
  }

  @override
  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

더 많은 정보는 [애니메이션 및 모션 위젯](https://flutter.io/docs/development/ui/widgets/animation), [애니메이션 튜토리얼](https://flutter.io/docs/development/ui/animations/tutorial) 및 [애니메이션 개요](https://flutter.io/docs/development/ui/animations)를 참고하십시오.

### 어떻게 화면에서 그리기를 합니까?

iOS에서는 `CoreGraphics`를 사용하여 화면에 선과 도형을 그립니다. Flutter는 `Canvas` 클래스에 기반한 다른 API를 제공하는데, `CustomPaint`와 `CustomPainter`라는 두 개의 다른 클래스가 그리는 것을 도와줍니다. `CustomPainter` 클래스는 캔버스에 그리는 알고리즘을 구현합니다.

Flutter에서 서명 페인터를 구현하는 방법을 배우기 위해 [스택오버플로우](https://stackoverflow.com/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter)에 있는 Collin의 답변을 참고하십시오.

```dart
class SignaturePainter extends CustomPainter {
  SignaturePainter(this.points);

  final List<Offset> points;

  void paint(Canvas canvas, Size size) {
    var paint = Paint()
      ..color = Colors.black
      ..strokeCap = StrokeCap.round
      ..strokeWidth = 5.0;
    for (int i = 0; i < points.length - 1; i++) {
      if (points[i] != null && points[i + 1] != null)
        canvas.drawLine(points[i], points[i + 1], paint);
    }
  }

  bool shouldRepaint(SignaturePainter other) => other.points != points;
}

class Signature extends StatefulWidget {
  SignatureState createState() => SignatureState();
}

class SignatureState extends State<Signature> {

  List<Offset> _points = <Offset>[];

  Widget build(BuildContext context) {
    return GestureDetector(
      onPanUpdate: (DragUpdateDetails details) {
        setState(() {
          RenderBox referenceBox = context.findRenderObject();
          Offset localPosition =
          referenceBox.globalToLocal(details.globalPosition);
          _points = List.from(_points)..add(localPosition);
        });
      },
      onPanEnd: (DragEndDetails details) => _points.add(null),
      child: CustomPaint(painter: SignaturePainter(_points), size: Size.infinite),
    );
  }
}
```

### 위젯의 불투명도는 어디에 있습니까?

iOS에서는 모든 것이 .opacity나 .alpha를 가지고 있습니다. Flutter에서는 이것을 달성하기 위해 Opacity 위젯 안에 위젯을 감싸야 합니다.

### 어떻게 커스텀 위젯을 만듭니까?

iOS에서는 기대하는 행동을 달성하는 메소드를 재정의하고 구현하기 위해 전형적으로 `UIView`의 서브클래스를 만들거나 이미 존재하는 뷰를 사용합니다. Flutter에서는 작은 위젯들을 확장하는 대신 [조합](https://flutter.io/docs/resources/technical-overview#everythings-a-widget)하여 커스텀 위젯을 만듭니다.

예를 들어 생성자에서 레이블을 취하는 `CustomButton`을 어떻게 만듭니까? `RaisedButton`을 확장하기보다는, `RaisedButton`을 레이블과 함께 조합하여 CustomButton을 생성하십시오.

```dart
class CustomButton extends StatelessWidget {
  final String label;

  CustomButton(this.label);

  @override
  Widget build(BuildContext context) {
    return RaisedButton(onPressed: () {}, child: Text(label));
  }
}
```

그리고 나서 다른 Flutter 위젯을 사용하는 것처럼 `CustomButton`을 사용하십시오.

```dart
@override
Widget build(BuildContext context) {
  return Center(
    child: CustomButton("Hello"),
  );
}
```

## 내비게이션*Navigation*

### 어떻게 페이지 간 내비게이션을 합니까?

iOS에서는 뷰 컨트롤러 간 이동을 위해 보여지는 뷰 컨트롤러의 스택을 관리하는 `UINavigationController`를 사용할 수 있습니다.

Flutter는 비슷한 구현을 가지고 있으며, `Navigator`와 `Routes`를 사용합니다. `Route`는 앱의 "화면"이나 "페이지"의 추상 표현이며, `Navigator`는 루트를 관리하는 [위젯](https://flutter.io/docs/resources/technical-overview#everythings-a-widget)입니다. 루트는 대략 `UIViewController`와 매핑됩니다. 내비게이터는 iOS의 `UINavigationController`와 비슷한 방법으로 작동합니다. 내비게이터는 뷰로 이동하기 원하는지, 되돌아가기 원하는지에 따라 루트를 `push()`하고 `pop()`할 수 있습니다.

페이지 간 이동을 위한 두 가지 선택지가 있습니다.

- 루트 이름의 `Map`을 명시하기 (MaterialApp)
- 직접적으로 루트로 이동하기 (WidgetApp)

다음의 예시는 Map을 만듭니다.

```dart
void main() {
  runApp(MaterialApp(
    home: MyAppHome(), // '/' 루트route가 됨
    routes: <String, WidgetBuilder> {
      '/a': (BuildContext context) => MyPage(title: 'page A'),
      '/b': (BuildContext context) => MyPage(title: 'page B'),
      '/c': (BuildContext context) => MyPage(title: 'page C'),
    },
  ));
}
```

`Navigator`에서 그 이름을 `push`하여 루트로 이동합니다.

```dart
Navigator.of(context).pushNamed('/b');
```

Flutter에서 `Navigator` 클래스는 라우팅을 처리하며 스택에 푸시한 루트에서 결과를 다시 얻는 것에 사용됩니다. 이것은 `push()`에 의해 반환된 `Future`에서 `await`하여 이루어집니다.

예를 들어 사용자가 그들의 위치를 선택하는 것을 허락하는 'location' 루트를 시작하기 위해 다음과 같이 작성할 것입니다.

```dart
Map coordinates = await Navigator.of(context).pushNamed('/location');
```

그리고 나서 'location' 루트에서 사용자가 그들의 위치를 일단 선택하면, 결과와 함께 스택에서 `pop()`합니다.

```dart
Navigator.of(context).pop({"lat":43.821757,"long":-79.226392});
```

### 어떻게 다른 앱으로 이동합니까?

iOS에서는 사용자에게 또다른 애플리케이션을 전달하기 위해 특정 URL 스킴을 사용합니다. 시스템 레벨 앱에서 스킴은 앱에 의존합니다. Flutter에서 이 기능을 구현하기 위해 네이티브 플랫폼 통합을 생성하거나, `url_launcher`와 같은 [플러그인](https://pub.dartlang.org/flutter/)을 사용하십시오.

### 어떻게 iOS의 네이티브 뷰 컨트롤러로 돌아갑니까?

Dart 코드에서 `SystemNavigator.pop()`을 호출하는 것은 다음의 iOS 코드를 발동합니다.

```objective-c
UIViewController* viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
if ([viewController isKindOfClass:[UINavigationController class]]) {
    [((UINavigationController*)viewController) popViewControllerAnimated:NO];
}
```

```swift
let viewController = UIApplication.shared.keyWindow?.rootViewController
if viewController is UINavigationController {
    (viewController as? UINavigationController)?.popViewController(animated: false)
}
```

저것이 당신이 원하는 것이 아니라면, 임의의 iOS 코드를 발동하기 위해 당신 자신의 [플랫폼 채널](https://flutter.io/docs/development/platform-integration/platform-channels)을 생성할 수 있습니다.

## 스레딩 및 비동기 처리*Threading & Asynchronicity*

### 어떻게 비동기적으로 작동하는 코드를 작성합니까?

Dart는 싱글 스레드 실행 모델을 가지고 있으며, `Isolate`(Dart 코드를 또다른 스레드에서 실행하는 방법), 이벤트 루프 및 비동기 프로그래밍을 지원합니다. `Isolate`를 사용하지 않는다면 Dart 코드는 메인 UI 스레드에서 작동하며 이벤트 루프에 의해 구동됩니다. Flutter의 이벤트 루프는 iOS의 메인 루프와 동일합니다. 즉, 메인 스레드에 부착된 `Looper`인 것입니다.

Dart의 싱글 스레드 모델은 모든 것이 UI를 얼어붙게 만드는 원인이 되는 블로킹 작업으로 실행해야 할 필요가 있다는 것을 의미하지 않습니다. 대신 `async/await`와 같은 Dart 언어가 제공하는 비동기 기능을 사용하여 비동기 작업을 수행하십시오.

예를 들어 `async/await`를 사용하고 Dart가 무거운 것을 들게 하여 UI가 멈추지 않고 네트워크 코드를 실행하게 할 수 있습니다.

```dart
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
```

`await`된 네트워크 호출이 완료되면 `setState()`를 호출하여 UI를 업데이트하십시오. 그러면 위젯의 서브트리가 다시 만들어지고 데이터가 업데이트됩니다.

다음의 예시는 비동기적으로 데이터를 로드하고 그것을 `ListView` 안에 보여줍니다.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();

    loadData();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView.builder(
          itemCount: widgets.length,
          itemBuilder: (BuildContext context, int position) {
            return getRow(position);
          }));
  }

  Widget getRow(int i) {
    return Padding(
      padding: EdgeInsets.all(10.0),
      child: Text("Row ${widgets[i]["title"]}")
    );
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

백그라운드에서 작업하는 것, Flutter가 iOS와 어떻게 다른지에 대해 더 많은 정보를 위해 다음 섹션을 참조하십시오.

### 어떻게 작업을 백그라운드 스레드로 보냅니까?

Flutter가 Node.js처럼 싱글 스레드이고 이벤트 루프에서 동작하기 때문에 스레드 관리나 백그라운드 스레드를 만드는 것에 대해 걱정할 필요는 없습니다. 디스크 접근이나 네트워크 호출과 같은 I/O 바인딩 작업을 하고 있다면, `async/await`를 안전하게 사용할 수 있으므로 문제될 것이 없습니다. 반면에 CPU를 바쁘게 만드는 컴퓨팅적으로 강도 높은 작업을 할 필요가 있다면, 이벤트 루프를 블로킹하는 것을 피하기 위해 `Isolate`로 작업을 옮기기를 원할 것입니다.

I/O 바인딩 작업을 위해 `async` 함수를 선언하고 함수 내부에서 오래 걸리는 작업을 `await`로 선언합니다.

```dart
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
```

이것이 전형적으로 네트워크나 데이터베이스 호출을 하는 방법입니다. 둘 모두 I/O 작업입니다.

그러나 거대한 양의 데이터를 처리하고 UI가 중단되는 경우가 있습니다. Flutter에서는 오래 걸리거나 컴퓨팅적으로 강도 높은 작업을 하기 위해 여러 개의 CPU 코어의 이점을 취하는 `Isolate`를 사용하십시오.

Isolate는 메인 실행 메모리 힙과 어떠한 메모리 공간도 공유하지 않는 분리된 실행 스레드입니다. 이것은 메인 스레드로부터 변수에 접근할 수 없거나, `setState()`를 호출하여 UI를 업데이트할 수 없다는 것을 의미합니다. Isolate는 그 이름에 충실하며, 예를 들어 정적 필드 폼에 있는 메모리를 공유할 수 없습니다.

다음의 예시는 간단한 Isolate에서 UI를 업데이트하기 위해 메인 스레드로부터 데이터를 공유하는 방법을 나타냅니다.

```dart
loadData() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // '메아리echo' isolate는 첫 번째 메세지로 그 SendPort를 보낸다
  SendPort sendPort = await receivePort.first;

  List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

  setState(() {
    widgets = msg;
  });
}

// isolate의 진입점
static dataLoader(SendPort sendPort) async {
  // 들어오는 메세지를 위한 RecevePort 열기
  ReceivePort port = ReceivePort();

  // 다른 isolate에게 이 isolate가 감지하는 포트를 알림
  sendPort.send(port.sendPort);

  await for (var msg in port) {
    String data = msg[0];
    SendPort replyTo = msg[1];

    String dataURL = data;
    http.Response response = await http.get(dataURL);
    // 파싱할 많은 양의 JSON
    replyTo.send(json.decode(response.body));
  }
}

Future sendReceive(SendPort port, msg) {
  ReceivePort response = ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
```

여기에서 `dataLoader()`는 그 자신의 분리된 실행 스레드에서 동작하는 `Isolate`입니다. Isolate 내에서 큰 JSON 파싱과 같은 더 많은 CPU에 집중되는 처리를 수행하거나 암호화 또는 시그널 처리와 같은 컴퓨팅적으로 강도 높은 수학을 수행할 수 있습니다.

아래에 있는 완전한 예시를 실행할 수 있습니다.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:async';
import 'dart:isolate';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    if (widgets.length == 0) {
      return true;
    }

    return false;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // '메아리echo' isolate가 첫 번째 메세지로 그 SendPort를 보냄
    SendPort sendPort = await receivePort.first;

    List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

    setState(() {
      widgets = msg;
    });
  }

// isolate의 진입점
  static dataLoader(SendPort sendPort) async {
    // 들어오는 메세지를 위해 ReceivePort를 열어둠
    ReceivePort port = ReceivePort();

    // 다른 isolate에게 이 isolate가 감지하고 있는 포트를 알림
    sendPort.send(port.sendPort);

    await for (var msg in port) {
      String data = msg[0];
      SendPort replyTo = msg[1];

      String dataURL = data;
      http.Response response = await http.get(dataURL);
      // 파싱할 많은 양의 JSON
      replyTo.send(json.decode(response.body));
    }
  }

  Future sendReceive(SendPort port, msg) {
    ReceivePort response = ReceivePort();
    port.send([msg, response.sendPort]);
    return response.first;
  }
}
```

### 어떻게 네트워크 요청을 합니까?

Flutter에서 네트워크 요청을 하는 것은 잘 알려진 [`http` 패키지](https://pub.dartlang.org/packages/http)를 사용하여 쉽게 할 수 있습니다. 이것은 일반적으로 스스로 구현할 수 있는 많은 네트워킹을 추상화하여 네트워크 호출을 간단하게 만듭니다.

`http` 패키지를 사용하기 위해 `subspec.yaml`에 의존성을 추가하십시오.

```yaml
dependencies:
  ...
  http: ^0.11.3+16
```

`async` 함수인 `http.get()`에 `await`로 호출하여 네트워크 호출을 하십시오.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
[...]
  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

### 어떻게 오래 걸리는 작업의 진행을 나타냅니까?

iOS에서는 백그라운드에서 오래 걸리는 작업을 수행하는 동안 대표적으로 `UIProgressView`를 사용합니다.

Flutter에서는 `ProgressIndicator` 위젯을 사용합니다. 불리언 플래그를 통해 렌더링될 때를 관리하여 코드로 진행 상황을 나타내십시오. 오래 걸리는 작업이 시작하기 전에 그 상태를 업데이트하고, 종료된 후 그것을 숨기는 것을 Flutter에게 알리십시오.

아래에 있는 예시에서 bulid 함수는 세 개의 다른 함수로 분리되어 있습니다. `showLoadingDialog()`가 `true`이면(`widgets.length == 0`일 때) `ProgressIndicator`를 렌더링합니다. 그렇지 않으면 네트워크 호출로부터 반환받은 데이터를 사용하여 `ListView`를 렌더링합니다.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    return widgets.length == 0;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
```

## 프로젝트 구조, 지역화, 의존성 및 에셋*Project structure, localization, dependencies and asests*

### 어떻게 Flutter에 이미지 에셋을 포함합니까? 여러 개의 해상도를 가질 때는 어떻게 합니까?

iOS는 이미지와 에셋을 별개의 아이템으로 취급하지만, Flutter 앱은 오직 에셋만을 가지고 있습니다. iOS에서 `Images.xcasset` 폴더에 위치하는 리소스는 Flutter에서는 assets 폴더에 위치합니다. iOS에서처럼 에셋은 이미지뿐만 아니라 어떠한 형식의 파일일 수 있습니다. 예를 들어 `my-assets` 폴더에 위치한 JSON 파일을 가질 수 있습니다.

> my-assets/data.json

`pubspec.yaml` 파일에 에셋을 선언하십시오.

```yaml
assets:
 - my-assets/data.json
```

그리고 나서 `AssetBundle`을 사용하여 코드에서 그것에 접근하십시오.

```yaml
import 'dart:async' show Future;
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('my-assets/data.json');
}
```

Flutter는 이미지를 위해 iOS와 같은 간단한 밀도 기반 포맷을 따릅니다. 이미지 에셋은 `1.0x, 2.0x, 3.0x` 또는 다른 배수를 가질 수 있습니다. 소위 `devicePixelRatio`는 단일 논리 픽셀에서 물리적 픽셀의 비율을 표현합니다.

에셋은 임의의 폴더에 위치합니다. Flutter에는 사전에 정의된 디렉토리 구조가 없기 때문입니다. `pubspec.yaml` 파일에 위치와 함께 에셋을 선언하면, Flutter가 그것을 집어올릴 것입니다.

예를 들어 Flutter 프로젝트에 `my_icon.png` 이미지를 추가하기 위해 `images`라고 불리는 임의의 폴더에 이미지를 저장하기로 결정할 것입니다. 기반 이미지(1.0x)를 `images` 폴더에 두십시오. 그러면 다른 것들은 적절한 비율 배수의 이름을 딴 서브 디렉토리에서 변형됩니다.

> images/my_icon.png

> images/2.0x/my_icon.png

> images/3.0x/my_icon.png

다음으로 이러한 이미지들을 `pubspec.yaml` 파일에 선언하십시오.

```yaml
assets:
  - images/my_icon.png
```

이제 `AssetImage`를 사용하여 이미지에 접근할 수 있습니다.

```dart
return AssetImage("images/a_dot_burr.jpeg");
```

또는 직접적으로 `Image` 위젯에서 접근할 수 있습니다.

```dart
@override
Widget build(BuildContext context) {
  return Image.asset("images/my_image.png");
}
```

[Flutter에서 에셋 및 이미지 추가하기](https://flutter.io/docs/development/ui/assets-and-images)에서 더 많은 세부 정보를 확인하십시오.

### 어디에 문자열을 저장합니까? 어떻게 지역화를 다룹니까?

`Localizable.strings` 파일을 가지고 있는 iOS와는 다르게 Flutter는 현재로서는 문자열을 다루는 기능을 하는 시스템을 가지고 있지 않습니다. 현 시점에서 가장 좋은 방법은 클래스에서 정적 필드로 사본 텍스트를 선언하고 거기에서 접근하는 것입니다. 예를 들어 아래와 같습니다.

```dart
class Strings {
  static String welcomeMessage = "Welcome To Flutter";
}
```

다음과 같이 문자열에 접근할 수 있습니다.

```dart
Text(Strings.welcomeMessage)
```

기본적으로 Flutter는 그 문자열로 미국식 영어만을 지원합니다. 다른 언어를 지원할 필요가 있다면 `flutter_localizations` 패키지를 포함하십시오. 날짜 및 시간 포맷팅과 같은 지역화 장치를 사용하기 위해 Dart의 `intl` 패키지를 포함할 필요도 있을 수 있습니다.

```yaml
dependencies:
  # ...
  flutter_localizations:
    sdk: flutter
  intl: "^0.15.6"
```

`flutter_localizations` 패키지를 사용하려면 앱 위젯에 `localizationsDelegates` 및 `supportedLocales`를 명시하십시오.

```dart
import 'package:flutter_localizations/flutter_localizations.dart';

MaterialApp(
 localizationsDelegates: [
   // 여기에 앱에 특정한 지역화 델리게이트를 추가
   GlobalMaterialLocalizations.delegate,
   GlobalWidgetsLocalizations.delegate,
 ],
 supportedLocales: [
    const Locale('en', 'US'), // 영어
    const Locale('he', 'IL'), // 히브리어
    // ... 앱이 지원하는 다른 지역
  ],
  // ...
)
```

델리게이트는 실제 지역화된 값을 포함하는 반면에, `supportedLocales`는 앱이 지원하는 지역을 정의합니다. 위의 예시는 `MaterialApp`을 사용하므로 기반 위젯의 지역화된 값을 위해서 `GlobalWidgetsLocalizations`를, 머터리얼 위젯의 지역화를 위해서 `MaterialWidgetsLocalizations`를 모두 가지고 있습니다. 앱에서 `WidgetsApp`을 사용한다면 후자를 필요로 하지 않습니다. 이 두 개의 델리게이트는 기본값을 포함하고 있으나 그것들도 지역화되는 것을 원한다면, 자신의 앱의 지역화 가능한 사본을 위한 하나 또는 그 이상의 델리게이트를 제공해야 할 필요가 있음을 알아두십시오.

`WidgetsApp` 또는 `MaterialApp`은 초기화될 때 명시된 델리게이트와 함께 `Localizations` 위젯을 생성합니다. 디바이스의 현재 지역은 현재 컨텍스트(`Locale` 오브젝트의 형식 내)에서 `Localizations` 위젯에서 항상 접근 가능하며, `Window.locale`을 사용할 수도 있습니다.

지역화된 리소스에 접근하기 위해, 주어진 델리게이트에 의해 제공된 특정 지역화 클래스에 접근하기 위해 `Localizations.of()` 메소드를 사용하십시오. `intl_translation` 패키지를 사용하여 번역할 arb 파일로 번역 가능한 사본을 추출하고 `intl`과 함께 사용하기 위해 다시 앱으로 가져옵니다.

[국제화 가이드](https://flutter.io/docs/development/accessibility-and-localization/internationalization)에서 Flutter에서의 국제화 및 지역화에 대해 더 많은 정보를 확인하십시오. `intl` 패키지를 포함한 샘플 코드와 포함하지 않은 것을 포함합니다.

Flutter 1.0 beta 2 이전 Flutter에서 정의된 에셋은 네이티브 측에서 접근할 수 없었고, 반대로도 Flutter에서 에셋은 여러 개의 폴더에 분리되어 있기 때문에 네이티브 에셋과 리소스를 Flutter에서 접근할 수 없었음을 알아두십시오.

### Cocoapods와 동등한 것은 무엇입니까? 어떻게 의존성을 추가합니까?

iOS에서는 `Podfile`에 추가하는 것으로 의존성을 추가합니다. Flutter는 의존성을 다루기 위해 Dart의 빌드 시스템과 Pub 패키지 매니저를 사용합니다. 이 도구들은 네이티브 안드로이드 및 iOS 래퍼 앱의 빌드를 각각의 빌드 시스템에 위임합니다.

Flutter 프로젝트에 있는 iOS 폴더에 Podfile이 있으나, 플랫폼별 통합을 필요로 하는 네이티브 의존성을 추가할 때만 이것을 사용하십시오. 일반적으로 Flutter에 외부 의존성을 선언하기 위해 `pubspec.yaml`을 사용하십시오. Flutter를 위한 훌륭한 패키지를 찾는 좋은 장소는 [Pub](https://pub.dartlang.org/flutter/packages/)입니다.

## 뷰 컨트롤러*ViewControllers*

### Flutter에서 `ViewController`와 동등한 것은 무엇입니까?

iOS에서 `ViewController`는 UI의 일부분을 나타내며 화면이나 섹션에서 가장 흔하게 사용됩니다. 복잡한 UI를 만들기 위해 함께 구성되며 앱의 UI를 확장하는 것을 도와줍니다. Flutter에서 이 작업은 위젯에 속합니다. 내비게이션 섹션에서 언급했다시피, Flutter에서 화면은 위젯에 의해 나타내어지는데 "모든 것은 위젯"이기 때문입니다. 다른 화면이나 페이지를 나타내는 다른 `Route` 사이를 이동하거나, 동일한 데이터의 다른 상태 또는 렌더링 사이를 이동하기 위해 `Navigator`를 사용하십시오.

### 어떻게 iOS의 생명주기 이벤트를 감지합니까?

iOS에서는 `ViewController`의 메소드를 재정의하여 뷰 자체를 위한 생명주기 메소드를 획득하거나, `AppDelegate`에서 생명주기 콜백을 등록할 수 있습니다. Flutter에서 두 가지에 해당하는 개념이 없으나 대신 `WidgetsBinding` 옵저버에 연결하고 `didChangeAppLifecycleState()` 변경 이벤트를 수신하여 생명주기 이벤트를 수신할 수 있습니다.

관찰 가능한 생명주기 이벤트는 다음과 같습니다.

- `inactive` : 애플리케이션은 활성화되지 않은 상태에 있으며 사용자 입력을 받지 않습니다. 이 이벤트는 오직 iOS에서 동작하며, 안드로이드에는 이와 동등한 이벤트가 없습니다.
- `paused` : 애플리케이션은 현재 사용자에게 보여지고 있지 않으며 사용자 입력에 응답하지 않으나 백그라운드에서 동작하고 있습니다.
- `resumed` : 애플리케이션은 보여지고 있으며 사용자 입력에 응답하고 있습니다.
- `suspending` : 애플리케이션은 잠시 중단되었습니다. iOS 플랫폼에는 동등한 이벤트가 없습니다.

[`AppLifecycleStatus` 문서](https://docs.flutter.io/flutter/dart-ui/AppLifecycleState-class.html)를 참고하여 이러한 상태의 의미에 대해 더 많은 것을 확인하십시오.

## 레이아웃*Layouts*

### Flutter에서 `UITableView`나 `UICollectionView`와 동등한 것은 무엇입니까?

iOS에서는 `UITableView`나 `UICollectionView`에 목록을 보여주었을 것입니다. Flutter에서는 `ListView`를 사용하여 비슷하게 구현합니다. iOS에서 이러한 뷰는 행의 개수, 각 인덱스 경로의 셀, 셀의 크기를 결정하기 위헤 델리게이트 메소드를 가지고 있습니다.

Flutter의 불변하는 위젯 패턴 때문에 `ListView`에 위젯 리스트를 넘겨주고 Flutter는 스크롤이 빠르고 부드럽게 이루어지도록 합니다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(Padding(padding: EdgeInsets.all(10.0), child: Text("Row $i")));
    }
    return widgets;
  }
}
```

### 어떻게 어떤 리스트 아이템이 클릭되었는지 압니까?

iOS에서는 `tableView:didSelectRowAtIndexPath:` 델리게이트 메소드를 구현합니다. Flutter에서는 전달된 위젯이 제공하는 터치 핸들링을 사용합니다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(GestureDetector(
        child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Text("Row $i"),
        ),
        onTap: () {
          print('row tapped');
        },
      ));
    }
    return widgets;
  }
}

```

### 어떻게 `ListView`를 동적으로 업데이트합니까?

iOS에서는 리스트 뷰를 위해 데이터를 업데이트하고 `reloadData` 메소드를 사용하여 테이블 뷰나 컬렉션 뷰에게 알립니다.

Flutter에서는 `setState()` 내에 위젯 리스트를 업데이트하면 데이터가 시각적으로 변화하지 않음을 빠르게 확인할 것입니다. 이것은 `setState()`가 호출될 때 Flutter의 렌더링 엔진이 어떤 것이 변화되었는지 확인하기 위해 위젯 트리를 살펴보기 때문입니다. `ListView`에 도착하면 `==` 확인을 수행하며 두 개의 `ListView`가 동일한지 결정합니다. 어떠한 것도 변화하지 않았다면 어떠한 업데이트도 필요로 하지 않습니다.

`ListView`를 업데이트하기 위한 간단한 방법은 `setState()` 내부에 새로운 `List`를 생성하고 오래된 리스트의 데이터를 새로운 리스트로 복사하는 것입니다. 이 접근법은 간단하지만 다음 예시에서 나타나는 것처럼 거대한 데이터셋에는 추천되지 않습니다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: widgets),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
        padding: EdgeInsets.all(10.0),
        child: Text("Row $i"),
      ),
      onTap: () {
        setState(() {
          widgets = List.from(widgets);
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
```

리스트를 만드는 추천되고 능률적이며 효율적인 방법은 `ListView.Builder`를 사용하는 것입니다. 이 메소드는 동적 리스트나 매우 많은 양의 데이터를 취하는 리스트에 매우 좋습니다.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (BuildContext context, int position) {
          return getRow(position);
        },
      ),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
        padding: EdgeInsets.all(10.0),
        child: Text("Row $i"),
      ),
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
```

"ListView"를 생성하는 대신 두 가지 주요한 매개변수를 취하는 `ListView.Builder`를 생성하십시오. 리스트의 초기 길이 및 `ItemBuilder` 함수를 취합니다.

`ItemBuilder` 함수는 iOS의 테이블뷰 및 컬렉션뷰에 있는 `cellForItemAt` 델리게이트 메소드와 비슷합니다. 위치 정보를 취하고 그 위치에 렌더링하기 원하는 셀을 반환하기 때문입니다.

마지막으로 가장 중요한 것은 `onTap()` 함수가 더이상 리스트를 다시 생성하지 않고 추가한다는 점입니다.

### Flutter에서 `ScrollView`와 동등한 것은 무엇입니까?

iOS에서는 `ScrollView`에 뷰를 감싸 사용자가 필요하다면 컨텐츠를 스크롤할 수 있게 허용합니다.

Flutter에서 이러한 것을 하는 가장 쉬운 방법은 `ListView` 위젯을 사용하는 것입니다. 위젯을 수직 포맷으로 배치할 수 있기 때문에 `ScrollView` 및 iOS의 `TableView` 모두의 역할을 합니다.

```dart
@override
Widget build(BuildContext context) {
  return ListView(
    children: <Widget>[
      Text('Row One'),
      Text('Row Two'),
      Text('Row Three'),
      Text('Row Four'),
    ],
  );
}
```

[레이아웃 튜토리얼](https://flutter.io/docs/development/ui/widgets/layout)을 참고하여 Flutter에서 위젯을 배치하는 방법에 대해 더 많은 문서를 확인하십시오.

## 제스처 감지 및 터치 이벤트 다루기*Gesture detection and touch event handling*

### Flutter에서 어떻게 위젯에 클릭 리스터를 추가합니까?

iOS에서는 `GestureRecognizer`를 뷰에 부착하여 클릭 이벤트를 다룹니다. Flutter에서는 터치 리스너를 추가하는 두 가지 방법이 있습니다.

1. 위젯이 이벤트 탐지를 지원한다면 그것에 함수를 넘기고 함수 내에서 이벤트를 처리하십시오. 예를 들어 `RaisedButton` 위젯은 `onPressed` 매개변수를 가지고 있습니다.
```dart
@override
Widget build(BuildContext context) {
  return RaisedButton(
    onPressed: () {
      print("click");
    },
    child: Text("Button"),
  );
}
```

2. 위젯이 이벤트 탐지를 지원하지 않는다면 위젯을 GestureDetector 내에 감싸고 `onTap` 매개변수에 함수를 넘기십시오.
```dart
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: GestureDetector(
          child: FlutterLogo(
            size: 200.0,
          ),
          onTap: () {
            print("tap");
          },
        ),
      ),
    );
  }
}
```

### 어떻게 위젯에서 다른 제스처를 다룹니까?

`GestureDetector`를 사용하여 다음과 같은 많은 제스처를 수신할 수 있습니다.

- 탭
  - `onTapDown` : 탭을 유발할 수 있는 포인터가 화면의 특정 위치에 접촉되었습니다.
  - `onTapUp` : 탭을 유발하는 포인터가 화면의 특정 위치에 접촉하는 것을 멈추었습니다.
  - `onTap` : 탭이 발생하였습니다.
  - `onTapCancel` : 이전에 `onTapDown`을 유발한 포인터가 탭을 유발하지 않습니다.
- 더블 탭
  - `onDoubleTap` : 사용자가 연속적으로 빠르게 화면의 같은 위치를 두 번 탭했습니다.
- 길게 누르기
  - `onLongPress` : 포인터가 오랜 시간 동안 동일한 위치에서 화면과 접촉한 채로 남아 있습니다.
- 수직 드래그
  - `onVerticalDragStart` : 포인터가 화면과 접촉하였고 수직적으로 움직이기 시작할 것입니다.
  - `onVerticalDragUpdate` : 화면과 접촉 상태인 포인터가 수직 방향으로 더 움직였습니다.
  - `onVerticalDragEnd` : 이전에 화면과 접촉 상태에 있고 수직적으로 움직이고 있는 포인터가 더이상 화면과 접촉하지 않고, 화면과 접촉하는 것을 멈추었을 때 특정 속도로 움직이고 있었습니다.
- 수평 드래그
  - `onHorizontalDragStart` : 포인터가 화면과 접촉하였고 수평적으로 움직이기 시작할 것입니다.
  - `onHorizontalDragUpdate` : 화면과 접촉 상태인 포인터가 수평 방향으로 더 움직였습니다.
  - `onHorizontalDragEnd` : 이전에 화면과 접촉 상태에 있고 수평적으로 움직이고 있는 포인터가 더이상 화면과 접촉하지 않습니다.

다음의 예시는 더블 탭으로 Flutter 로고를 회전시키는 `GestureDetector`를 나타냅니다.

```dart
AnimationController controller;
CurvedAnimation curve;

@override
void initState() {
  controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
  curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: GestureDetector(
          child: RotationTransition(
            turns: curve,
            child: FlutterLogo(
              size: 200.0,
            )),
          onDoubleTap: () {
            if (controller.isCompleted) {
              controller.reverse();
            } else {
              controller.forward();
            }
          },
        ),
      ),
    );
  }
}
```

## 테마 및 텍스트*Theming and text*

### 어떻게 앱의 테마를 지정합니까?

기본적으로 Flutter는 머터리얼 디자인의 아름다운 구현이 포함되어 있습니다. 이것은 당신이 일반적으로 수행하는 많은 스타일링 및 테마 요구를 처리합니다.

앱에서 머터리얼 컴포넌트의 완전한 이점을 취하기 위해 애플리케이션의 진입점에 최상위 레벨 위젯인 MaterialApp을 선언하십시오. MaterialApp은 머터리얼 디자인을 구현하는 애플리케이션을 위해 흔히 요구되는 많은 위젯을 감싸는 편리한 위젯입니다. 그것은 머터리얼에 특정한 기능을 더하여 WidgetsApp을 기반으로 합니다.

하지만 Flutter는 어떠한 디자인 언어를 구현하는데 충분히 유연하고 표현적입니다. iOS에서는 [Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/)를 고수하는 인터페이스를 만들기 위해 [쿠퍼티노 라이브러리](https://docs.flutter.io/flutter/cupertino/cupertino-library.html)를 사용할 수 있습니다. [쿠퍼티노 위젯 갤러리](https://flutter.io/docs/development/ui/widgets/cupertino)에서 이러한 위젯의 전체 구성을 확인하십시오.

또한 앱 위젯으로 `WidgetApp`을 사용할 수 있는데, 이것은 몇몇 같은 기능을 제공하지만 `MaterialApp`만큼 풍부하지는 않습니다.

어떠한 자식 컴포넌트의 색상과 스타일을 커스터마이징하기 위해 `MaterialApp` 위젯에 `ThemeData` 오브젝트를 넘기십시오. 예를 들어 아래의 코드에서 기본 견본*primary swatch*는 파란색으로 설정되고 텍스트 선택 색상은 빨간색으로 설정됩니다.

```dart
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textSelectionColor: Colors.red
      ),
      home: SampleAppPage(),
    );
  }
}
```

### 어떻게 `Text` 위젯에 커스텀 폰트를 설정합니까?

iOS에서는 어떤 `ttf` 폰트 파일을 프로젝트에 불러오고 `info.plist` 파일에 참조를 생성합니다. Flutter에서는 폰트 파일을 폴더에 두고 `pubspec.yaml` 파일에서 그것을 참조합니다. 이미지를 불러오는 방법과 비슷합니다.

```yaml
fonts:
   - family: MyCustomFont
     fonts:
       - asset: fonts/MyCustomFont.ttf
       - style: italic
```

그리고 나서 `Text` 위젯에 폰트를 할당합니다.

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: Text(
        'This is a custom font text',
        style: TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
```

### 어떻게 `Text` 위젯의 스타일을 지정합니까?

폰트와 함께 `Text` 위젯에서 다른 스타일 요소를 커스터마이징할 수 있습니다. `Text` 위젯의 스타일 매개변수는 `TextStyle` 오브젝트를 취하는데, 다음과 같은 많은 매개변수를 커스터마이징할 수 있습니다.

- `color`
- `decoration`
- `decorationColor`
- `decorationStyle`
- `fontFamily`
- `fontSize`
- `fontStyle`
- `fontWeight`
- `hashCode`
- `height`
- `inherit`
- `letterSpacing`
- `textBaseline`
- `wordSpacing`

## 폼 입력*Form input*

### Flutter에서 폼은 어떻게 동작합니까? 어떻게 사용자의 입력을 받습니까?

Flutter가 별개의 상태와 함께 불변하는 위젯을 사용하는 방법을 감안할 때, 사용자 입력이 어떻게 그림에 들어맞는 것인지 궁금해할 수 있습니다. iOS에서는 보통 사용자 입력 또는 액션을 제출할 때 위젯에 현재 값을 쿼리합니다. Flutter에서는 어떻게 동작합니까?

사실 폼은 Flutter에 있는 모든 것들처럼 특별한 위젯으로 다루어집니다. `TextField`나 `TextFormField`를 가지고 있다면 사용자 입력을 받기 위해 `TextEditingController`를 제공할 수 있습니다.

```dart
class _MyFormState extends State<MyForm> {
  // 텍스트 컨트롤러를 생성하고 텍스트 필드의 현재 값을 받기 위해 사용합니다.
  final myController = TextEditingController();

  @override
  void dispose() {
    // 위젯을 폐기할 때 컨트롤러를 정리합니다.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Retrieve Text Input'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          controller: myController,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // 사용자가 버튼을 누를 때, 사용자가 텍스트 필드에 입력한 텍스트를 가지고 알림창을 보여줍니다.
        onPressed: () {
          return showDialog(
            context: context,
            builder: (context) {
              return AlertDialog(
                // TextEditingController를 사용하여 사용자가 입력한 텍스트를 받습니다.
                content: Text(myController.text),
              );
            },
          );
        },
        tooltip: 'Show me the value!',
        child: Icon(Icons.text_fields),
      ),
    );
  }
}
```

[Flutter 쿡북](https://flutter.io/docs/cookbook)에 있는 [텍스트 필드의 값 받기](https://flutter.io/docs/cookbook/forms/retrieve-input)에서 더 많은 정보와 모든 코드 리스트를 확인할 수 있습니다.

### 텍스트 필드의 플레이스홀더와 동등한 것은 무엇입니까?

Flutter에서는 `Text` 위젯을 위한 생성자의 decoration 매개변수에 `InputDecoration` 오브젝트를 추가하여 필드에 "힌트"나 플레이스홀더 텍스트를 쉽게 보여줄 수 있습니다.

```dart
body: Center(
  child: TextField(
    decoration: InputDecoration(hintText: "This is a hint"),
  ),
)
```

### 어떻게 유효성 에러를 보여줍니까?

"힌트"와 마찬가지로 `InputDecoration` 오브젝트를 `Text` 위젯의 생성자의 decoration 매개변수에 넘겨줍니다.

그러나 에러를 보여주면서 시작하고 싶지는 않을 것입니다. 대신 사용자가 유효하지 않은 데이터를 입력했을 때 그 상태를 업데이트하고 새로운 `InputDecoration` 오브젝트를 넘겨주십시오.

```dart
class SampleApp extends StatelessWidget {
  // 이 위젯은 애플리케이션의 루트입니다.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  String _errorText;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: TextField(
          onSubmitted: (String text) {
            setState(() {
              if (!isEmail(text)) {
                _errorText = 'Error: This is not an email';
              } else {
                _errorText = null;
              }
            });
          },
          decoration: InputDecoration(hintText: "This is a hint", errorText: _getErrorText()),
        ),
      ),
    );
  }

  _getErrorText() {
    return _errorText;
  }

  bool isEmail(String emailString) {
    String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = RegExp(emailRegexp);

    return regExp.hasMatch(emailString);
  }
}
```



## 하드웨어, 서드 파티 서비스 및 플랫폼과 상호작용하기*Interacting with hardware, third party services and the platform*

### 어떻게 플랫폼, 플랫폼 네이티브 코드와 상호작용합니까?

Flutter는 기초를 이루는 플랫폼 위에서 직접적으로 코드를 실행하지 않습니다. 대신 Flutter 앱을 구성하는 Dart 코드가 디바이스에서 네이티브하게 동작하며, 플랫폼이 제공하는 SDK를 "sidestepping"합니다. 예를 들어 그것은 Dart에서 네트워크 요청을 수행할 때 Dart의 컨텍스트에서 직접적으로 동작한다는 것을 의미합니다. 네이티브 앱을 작성할 때 일반적으로 활용하는 안드로이드 또는 iOS API를 사용하지 않습니다. Flutter 앱은 여전히 네이티브 앱의 `ViewController` 내에서 뷰로서 호스팅되지만, `ViewController` 자체나 네이티브 프레임워크에 직접 접근할 수 없습니다.

이것은 Flutter 앱이 그러한 네이티브 API나 어떠한 네이티브 코드와도 상호작용할 수 없다는 것을 의미하지 않습니다. Flutter는 [플랫폼 채널](https://flutter.io/docs/development/platform-integration/platform-channels)을 제공하며 그것은 Flutter의 뷰를 호스팅하는 `ViewController`와 함께 소통하고 데이터를 교환합니다. 플랫폼 채널은 본질적으로 Dart 코드를 호스트 `ViewController` 및 실행중인 iOS 프레임워크와 연결하는 비동기 메세징 메커니즘입니다. 예를 들어 플랫폼 채널을 네이티브 측에 있는 메소드를 실행하거나 디바이스의 센서로부터 몇몇 데이터를 가져오기 위해 사용할 수 있습니다.

플랫폼 채널을 직접적으로 사용하는 것에 외에도, 특정한 목표를 위해 네이티브 및 Dart 코드를 캡슐화하는 다양한 사전 제작된 플러그인을 사용할 수 있습니다. 예를 들어 통합을 작성할 필요 없이 Flutter에서 직접 카메라 롤과 디바이스 카메라에 접근하는 플러그인을 사용할 수 있습니다. 플러그인은 Dart 및 Flutter의 오픈 소스 패키지 저장소 [Pub](https://pub.dartlang.org/)에서 찾을 수 있습니다. 몇몇 패키지는 iOS 또는 안드로이드, 또는 둘 모두에 대한 네이티브 통합을 지원합니다.

니즈와 맞는 플러그인을 Pub에서 찾을 수 없다면 [직접 작성](https://flutter.io/docs/development/packages-and-plugins/developing-packages)하여 [Pub에 배포](https://flutter.io/docs/development/packages-and-plugins/developing-packages#publish)할 수 있습니다.

### 어떻게 GPS 센서에 접근합니까?

`geolocator` 커뮤니티 플러그인을 사용하십시오.

### 어떻게 카메라에 접근합니까?

`image_picker` 플러그인이 카메라 접근에 있어서 가장 유명합니다.

### 어떻게 Facebook을 사용하여 로그인 합니까?

Facebook을 사용하여 로그인하기 위해 `flutter_facebook_login` 커뮤니티 플러그인을 사용하십시오.

### 어떻게 Firebase의 기능을 사용합니까?

대부분의 Firebase 기능은 [퍼스트 파티 플러그인](https://pub.dartlang.org/flutter/packages?q=firebase)으로 다루어집니다. 이러한 플러그인은 퍼스트 파티 통합이며 Flutter 팀에 의해 유지보수됩니다.

- `firebase_admob` : Firebase AdMob
- `firebase_analytics` : Firebase Analytics
- `firebase_auth` : Firebase Auth
- `firebase_core` : Firebase의 Core 패키지
- `firebase_database` : Firebase RTDB
- `firebase_storage` : Firebase Cloud Storage
- `firebase_messaging` : Firebase Messaging (FCM)
- `cloud_firestore` : Firebase Cloud Firestore

또한 퍼스트 파티 플러그인에 의해 직접적으로 다루어지지 않은 영역을 다룬 서드 파티 Firebase 플러그인을 Pub에서 찾을 수 있습니다.

### 어떻게 커스텀 네이티브 통합을 빌드합니까?

Flutter나 그 커뮤니티 플러그인이 누락된 플랫폼에 특화된 기능이 있다면 [개발 중인 패키지 및 플러그인](https://flutter.io/docs/development/packages-and-plugins/developing-packages) 페이지에 따라 직접 빌드할 수 있습니다.

Flutter의 플러그인 아키텍처는 안드로이드에서 이벤트 버스를 사용하는 것과 매우 비슷합니다. 메세지를 실행하고 수신자가 처리하여 결과를 다시 내보냅니다. 이 경우 수신자는 안드로이드나 iOS의 네이티브 측에서 실행되는 코드입니다.

## 데이터베이스 및 로컬 저장소*Databases and local storage*

### Flutter에서 어떻게 `UserDefaults`에 접근합니까?

iOS에서는 `UserDefaults`라고 알려져 있는 프로퍼티 리스트를 사용하여 키-값 쌍의 컬렉션을 저장할 수 있습니다.

Flutter에서는 [Shared Preferences 플러그인](https://pub.dartlang.org/packages/shared_preferences)을 사용하여 동등한 기능에 접근하십시오. 이 플러그인은 `UserDefaults` 및 안드로이드에서 동등한 `SharedPreferences`의 기능을 모두 감쌉니다.

### Flutter에서 CoreData와 동등한 것은 무엇입니까?

iOS에서는 구조화된 데이터를 저장하기 위해 CoreData를 사용할 수 있습니다. 이것은 단순히 SQL 데이터베이스의 상위에 있는 층이므로 모델과 관련된 쿼리를 더 쉽게 만들 수 있습니다.

Flutter에서는 `SQFlite` 플러그인을 사용하여 이 기능에 접근하십시오.

## 노티피케이션*Notifications*

### 어떻게 푸시 알림을 설정합니까?

iOS에서는 푸시 알림을 허용하기 위해 개발자 포털에서 앱을 등록할 필요가 있습니다.

Flutter에서는 `firebase_messaging` 플러그인을 사용하여 이 기능에 접근하십시오.

`firebase_messaging` 플러그인 문서를 참고하여 Firebase Cloud Messaging API를 사용하기 위한 더 많은 정보를 확인하십시오.
