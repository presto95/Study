[원문](https://github.com/raywenderlich/swift-style-guide)

현재 Swift 4.2 기준

## 정확성

코드가 경고 없이 컴파일되게 하라. 이 규칙은 문자열 리터럴 대신 `#selector` 타입을 사용하는 것과 같은 많은 스타일 결정을 알려준다.

## 네이밍

서술적이고 일관된 네이밍은 소프트웨어를 읽고 이해하기 쉽게 한다. API Design Guidelines에 기술된 Swift의 네이밍 컨벤션을 사용하라. 몇몇 주요 비결은 다음과 같다.

- 호출 시점에서의 명확함
- 간결함보다 명확함을 우선시하기
- 캐멀 케이스 사용하기 (스네이크 케이스 사용하지 않기)
- 타입 (및 프로토콜) 에 대하여 대문자, 다른 모든 것들에 대해서 소문자 사용하기
- 불필요한 단어를 제외하고 필요한 모든 단어를 포함시키기
- 타입이 아닌 역할에 기반한 이름 사용하기
- 이따금 약타입 정보를 보상하기
- 유창한 사용성
- `make` 로 시작하는 팩토리 메소드 이름
- 메소드의 사이드 이펙트에 대하여 네이밍하기
  - 동사 메소드 변형하지 않는 버전에 대하여 -ed나 -ing가 뒤따라옴
  - 명사 메소드는 변형되는 버전에 대하여 `formXXX` 규칙을 사용
  - 불리언 타입은 주장과 같이 읽혀야 함
  - *무엇이 어떤 것인지* 묘사하는 프로토콜은 명사로 읽혀야 함
  - *능력*을 묘사하는 프로토콜은 -able 또는 -ible로 끝나야 함
- 전문가를 놀라게 하고 초보자를 혼란스럽게 하는 용어를 사용하지 않기
- 일반적으로 약어를 사용하지 않기
- 이름에 대해서 선례를 사용하기
- 전역 함수보다 메소드 및 프로퍼티 선호하기
- 두문자어는 공통되게 대문자 또는 소문자 사용하기
- 반환 타입에 대한 오버로딩 사용하지 않기
- 문서로 제공될 수 있는 좋은 매개변수 이름 선택하기
- 메소드 이름 안에 매개변수의 이름을 포함하는 대신 첫 번째 매개변수에 이름 짓기. Delegate 하에서 언급되는 것은 제외함
- 클로저 및 튜플 매개변수에 이름 붙이기
- 기본 매개변수의 이점 취하기

### 산문

메소드를 산문 형식으로 언급할 때 명백해지는 것이 중요하다. 메소드 이름을 언급할 때, 가능한 한 가장 간단한 형식을 사용하라.

1. 매개변수 없이 메소드 이름 작성하기.
   - Next, you need to call `addTarget`.
2. 전달인자 레이블과 함께 메소드 이름 작성하기.
   - Next, you need to call `addTarget(_:action:)`.
3. 전달인자 레이블과 타입과 함께 메소드 이름 작성하기.
   - Next, you need to call `addTarget(_: Any?, action: Selector?)`.

`UIGestureRecognizer`를 사용한다면, 1의 경우가 명백하고 선호된다.

**프로 팁** : Xcode의 점프 바를 사용하여 전달 인자 레이블과 함께 메소드를 찾을 수 있다. 동시에 많은 키를 누르는 것을 잘 한다면, 메소드 이름에 커서를 두고 Shift-Control-Option-Command-C를 눌러라. 그러면 Xcode는 기꺼이 클립보드에 그 시그니쳐를 복사할 것이다.

### 클래스 접두사

Swift의 타입은 그것을 포함하는 모듈에 의해 자동으로 네임스페이스가 지어지며 클래스 접두사를 더하지 않아도 된다. 다른 모듈에 있는 두 이름이 충돌한다면 모듈 이름을 타입 이름 앞에 붙여 모호함을 없앨 수 있다. 그러나, 혼란스러울 수 있는 곳에만 모듈 이름을 지정하라. 이는 매우 드물 것이다.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### Delegate

커스텀 델리게이트 메소드를 만들 때, 이름 없는 첫 번째 매개변수는 델리게이트의 소스가 되어야 한다. UIKit은 이와 관련된 수많은 예제를 포함한다.

### 선호:

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldRealod(_ namePickerView: NamePickerView) -> Bool
```

#### 선호하지 않음:

```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### 타입 추론 가능한 컨텍스트 사용

더 짧고 분명한 코드를 작성하기 위해 컴파일러의 타입 추론 컨텍스트를 사용하라.

#### 선호:

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

#### 선호하지 않음:

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### 제네릭

제네릭 타입 매개변수는 설명적이어야 하며, 대문자 캐멀 케이스를 사용한다. 타입 이름이 의미 있는 관계나 역할을 갖지 않는다면, `T`, `U`, `V`와 같은 전통적인 단일 대문자 하나를 사용하라.

#### 선호:

```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

#### 선호하지 않음:

```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### 언어

Apple의 API와 일치시키기 위해 미국식 영어 스펠링을 사용하라.

#### 선호:

```swift
let color = "red"
```

#### 선호하지 않음:

```swift
let colour = "red"
```

## 코드 구성

논리적 블록 또는 기능 안에 코드를 구성하기 위해 익스텐션을 사용하라. 각각의 익스텐션은 `// MARK: - ` 주석과 함께하여 잘 구성되도록 해야 한다.

### 프로토콜 준수

특히, 모델에 프로토콜 준수를 추가할 때, 프로토콜 메소드에 대하여 분리된 익스텐션을 추가하도록 하라. 이는 관련 메소드가 프로토콜과 함께 묶이도록 하며 관련 메소드와 함께 클래스에 프로토콜을 추가하여 지시 사항을 단순하게 할 수 있다.

#### 선호:

```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

#### 선호하지 않음:

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

컴파일러는 파생된 클래스에서 프로토콜 준수를 다시 선언하지 못하게 하기 때문에, 기반 클래스의 익스텐션 그룹을 복제하는 것은 항상 필요한 것은 아니다. 이는 특히 파생된 클래스가 최종 클래스이며 적은 수의 메소드가 재정의될 때 그렇다. 익스텐션 그룹을 보존해야 할 때는 작성자에게 달려 있다.

UIKit의 뷰 컨트롤러에 대하여, 라이프 사이클끼리, 커스텀 접근자끼리, IBAction끼리 분리된 클래스 익스텐션에 정의하는 것을 고려하라.

### 사용되지 않는 코드

Xcode의 템플릿 코드와 플레이스홀더 주석을 포함하여, 사용되지 않는 코드는 제거될 필요가 있다. 튜토리얼이나 책이 주석 처리된 코드를 사용자가 사용하도록 하는 경우는 예외다.

구현이 단순하게 수퍼 클래스를 호출하는, 튜토리얼과 직접적으로 관계되지 않은 야심찬 메소드도 제거되어야 한다. 이는 비거나 사용되지 않는 UIApplicationDelegate 메소드를 포함한다.

#### 선호:

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

#### 선호하지 않음:

```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```

### 최소한의 import

소스 파일이 요구하는 모듈만 import하라. 예를 들어, `Foundation`을 import하는 것이 충분하다면 `UIKit`을 import하지 마라. 비슷하게, `UIKit`을 import해야 한다면 `Foundation`을 import하지 마라.

#### 선호:

```swift
import UIKit
var view: UIView
var deviceModels: [String]
```

#### 선호:

```swift
import Foundation
var deviceModels: [String]
```

#### 선호하지 않음:

```swift
import UIKit
import Foundation
var view: UIView
var deviceModels: [String]
```

#### 선호하지 않음:

```swift
import UIKit
var deviceModels: [String]
```

## 빈칸

- 공간을 보존하고 줄바꿈을 막기 위해 탭 대신 2 스페이스를 사용하여 들여쓰기하라. Xocde의 Preference와 Project settings에서 이를 설정하라.
- 메소드의 괄호와 다른 괄호 (`if` / `else` / `switch` / `while ` 등) 는 항상 해당 선언문과 같은 라인에서 열리나 새로운 라인에서 닫힌다.
- 팁 : 코드를 선택하고 Control-I를 입력하여 들여쓰기를 다시 할 수 있다. 몇몇 Xcode 템플릿 코드는 4개의 스페이스를 가진 탭이 하드코딩되어 있으므로, 그것을 고치기 위한 좋은 방법이 된다.

#### 선호:

```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

#### 선호하지 않음:

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

- 시각적 명백함과 구성을 돕기 위해 메소드 사이에 정확히 하나의 빈 라인이 들어가야 한다. 메소드 안에 있는 빈 라인은 상관성에 의해 분리되어야 하나, 너무 많은 섹션이 하나의 메소드에 들어가는 것은 여러 개의 메소드로 쪼개야 한다는 것을 의미할 수 있다.
- 여는 괄호 다음 또는 닫는 괄호 이전에 빈 라인이 있으면 안된다.
- 콜론은 좌측에 빈칸이 없고 우측에 하나의 빈칸이 있어야 한다. 삼항 연산자 `? : `, 빈 딕셔너리 `[:]`, `#selector` 구문 `addTarget(_:action:)`은 예외다.

#### 선호:

```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

#### 선호하지 않음:

```swift
class TestDatabase: Database {
  var data:[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

- 긴 라인의 경우 70문자 정도에서 줄바꿈이 일어나야 한다. 견고한 제한은 의도적으로 지정되지 않는다.
- 라인의 끝에 빈칸을 붙이지 않도록 하라.
- 각 파일의 끝에 하나의 새 라인 문자를 추가하라.

## 주석

필요하다면 **왜** 특정 코드 조각이 무엇을 하는지 설명하기 위해 주석을 사용하라. 주석은 최신 상태가 유지되어야 하며 그렇지 않으면 삭제되어야 한다.

코드는 가능하다면 그 자체가 문서의 역할을 할 수 있어야 하므로, 코드 안에 블록 주석을 사용하지 마라. 문서 생성에 사용되는 주석은 예외다.

C 스타일의 주석 (`/* … */`) 을 사용하지 마라. 두 개 또는 세 개의 슬래시를 사용하라.

## 클래스 및 구조체

### 둘 중 어느 것을 사용하는가?

기억하라. 구조체는 값 의미론을 갖는다. 동일함을 갖지 않는 것에 대하여 구조체를 사용하라. [a, b, c]를 포함하는 배열은 [a, b, c]를 포함하는 또다른 배열과 정말로 같으며, 완벽하게 상호 교환 가능하다. 첫 번째 또는 두 번째 배열 중 어느 것을 사용하는지는 중요하지 않다. 그것들은 정확히 같은 것을 나타내기 때문이다. 이것이 배열이 구조체인 이유다.

클래스는 참조 의미론을 갖는다. 동일함을 갖거나 특정한 생명 주기를 갖는 것에 대하여 클래스를 사용하라. 두 개의 사람 오브젝트는 두 개의 다른 것이므로 사람을 모델링할 때 클래스를 사용할 것이다. 단지 두 사람이 같은 이름과 같은 생일을 가졌다고 해서, 이들이 같은 사람이라는 것을 의미하지는 않는다. 하지만 사람의 생일은 1950년 3월 3일의 날짜가 1950년 3월 3일의 날짜를 갖는 다른 날짜 오브젝트와 같기 때문에 구조체가 되어야 할 것이다. 날짜 그 자체는 동일함을 갖지 않는다.

때때로, 어떠한 것은 구조체여야 하나 `AnyObject`를 준수해야 하거나 역사적으로 이미 클래스로 모델링되어 있습니다 (`NSDate`, `NSSet`). 가능한 한 다음의 가이드라인을 따르도록 하십시오.

### 정의 예시

다음은 잘 정의된 클래스 정의의 예시다.

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  override func area() -> Double {
    return Double.pi * radius * radius
  }
}

extension Circle: CustomStringConvertible {
  var description: String {
    return "center = \(centerString) area = \(area())"
  }
  private var centerString: String {
    return "(\(x),\(y))"
  }
}
```

위에 있는 예시는 다음의 스타일 가이드라인을 입증합니다.

- 프로퍼티, 변수, 상수, 인자 선언 및 다른 선언문에 타입을 명시하며, 콜론 뒤에 빈칸이 있으나 앞에는 없다. 예) `x: Int`, `Circle: Shape`
- 공통의 목적과 컨텍스트를 공유한다면 한 줄에 여러 개의 변수와 구조체를 정의하라.
- 접근자 및 설정자 정의와 프로퍼티 감시자에 대하여 들여쓰기하라.
- 이미 기본값일 때 `internal`과 같은 지정자를 추가하지 마라. 비슷하게, 메소드를 재정의할 때 접근 지정자를 반복하지 마라.
- 익스텐션에 여분의 기능을 구성하라.
- `private` 접근 지정자를 사용하여 `centerString`과 같은 공유되지 않고 세부 구현을 익스텐션 안에 숨겨라.

### Self 사용

Swift는 오브젝트의 프로퍼티에 접근하거나 메소드를 호출하기 위해 `self`를 요구하지 않으므로, 간결함을 위해 `self`를 사용하지 마라.

컴파일러가 필요로 할 때만 self를 사용하라 (`@escaping` 클로저 내부 또는 프로퍼티와 인자를 모호하게 하는 이니셜라이저 내부). 즉, `self` 없이 컴파일 된다면 그것을 생략하라.

### 연산 프로퍼티

간결함을 위해, 연산 프로퍼티가 읽기 전용이라면 get절을 생략하라. get절은 오직 set절이 제공될 때만 요구된다.

#### 선호:

```swift
var diameter: Double {
  return radius * 2
}
```

#### 선호하지 않음:

```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

### Final

튜토리얼에서 클래스나 멤버를 `final`로 표시하는 것은 주요 토픽과 동떨어지게 할 수 있으며 필요로 하지 않는다. 그럼에도 불구하고, `final`을 사용하는 것은 때때로 당신의 의도를 명확하게 할 수 있으며 비용적으로도 가치 있다. 아래의 예제에서, `Box`는 특정 목적을 가지고 있으며 파생된 클래스에서의 커스터마이징은 의도되지 않는다. 이를 명백하기 위해 `final`을 표시한다.

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
  let value: T
  init(_ value: T) {
    self.value = value
  }
}
```

## 함수 선언

여는 괄호를 포함하여 함수 선언을 한 줄에 짧게 유지하라.

```swift
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```

긴 시그니쳐를 갖는 함수에 대하여, 각각의 매개변수를 새로운 라인에 작성하고 각각의 라인에 여분의 인덴트를 추가하라.

```swift
func reticulateSplines(
  spline: [Double], 
  adjustmentFactor: Double,
  translateConstant: Int, comment: String
) -> Bool {
  // reticulate code goes here
}
```

입력이 없다는 것을 나타내기 위해 `(Void)`를 사용하지 마라. 간단하게 `()`를 사용하라. 클로저 및 함수의 출력에 대하여 `()` 대신 `Void`를 사용하라.

#### 선호:

```swift
func updateConstraints() -> Void {
  // magic happens here
}

typealias CompletionHandler = (result) -> Void
```

#### 선호하지 않음:

```swift
func updateConstraints() -> () {
  // magic happens here
}

typealias CompletionHandler = (result) -> ()
```

## 함수 호출

호출 지역에서 함수 선언의 스타일을 반영하라. 한 줄에 알맞는 호출은 다음과 같이 작성되어야 한다.

```swift
let success = reticulateSplines(splines)
```

호출 지역에 줄바꿈이 일어나야 한다면, 각각의 매개변수를 새로운 라인에 작성하고, 하나의 추가적인 들여쓰기 수준을 적용하라.

```swift
let success = reticulateSplines(
  spline: splines,
  adjustmentFactor: 1.3,
  translateConstant: 2,
  comment: "normalize the display"
)
```

## 클로저 표현식

인자 리스트의 끝에 하나의 클로저 표현식이 있을 때만 후행 클로저 문법을 사용하라. 클로저 매개변수에 설명적인 이름을 주어라.

#### 선호:

```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```

#### 선호하지 않음:

```swift
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

컨텍스트가 명백한 곳에서 하나의 표현식이 있는 클로저에 대하여, 암시적 반환을 사용하라.

```swift
attendeeList.sort { a, b in
  a > b
}
```

후행 클로저를 사용하여 연결된 메소드는 컨텍스트에서 명백하고 읽기 쉬워야 한다. 빈칸, 줄바꿈, 이름 있는 인자 또는 익명 인자를 사용하는 때에 대한 결정은 작성자에게 달려 있다.

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```

## 타입

가능하다면 항상 Swift 네이티브 타입과 표현식을 사용하라. Swift는 Objective-C 브릿지를 제공하므로 필요하다면 여전히 모든 메소드를 사용할 수 있다.

#### 선호:

```swift
let width = 120.0
let widthString = "\(width)"
```

#### 덜 선호함:

```swift
let width = 120.0
let widthString = (width as NSNumber).stringValue
```

#### 선호하지 않음:

```swift
let width: NSNumber = 120.0
let widthString: NSString = width.stringValue
```

드로잉 코드에서, `CGFloat`를 사용하여 너무 많은 변환을 피하는 것으로 코드를 간결하게 하라.

### 상수

상수는 `let` 키워드, 변수는 `var` 키워드를 사용하여 정의된다. 변수의 값이 변화하지 않을 것이라면 항상 `var` 대신 `let`을 사용하라.

**팁** : 모든 것이 `let`을 사용하도록 하고 컴파일러가 불평할 때에만 `var`를 사용하도록 바꾸는 것은 좋은 테크닉이다.

타입 프로퍼티를 사용하여 해당 타입의 인스턴스가 아닌, 타입 자체에 상수를 정의할 수 있다. 타입 프로퍼티를 상수로 선언하기 위해 `static let`을 사용한다. 이러한 방식으로 선언된 타입 프로퍼티는 일반적으로 전역 상수보다 선호되는데, 인스턴스 프로퍼티로부터 구별되기 쉽기 때문이다.

#### 선호:

```swift
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

**알아두기** : 케이스가 없는 열거형을 사용하는 것의 이점은 실수로 인스턴스화될 수 없으며 순수하게 네임스페이스로서 작동한다는 것이다.

#### 선호하지 않음:

```swift
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // what is root2?
```

### 정적 메소드 및 변수 타입 프로퍼티

정적 메소드 및 타입 프로퍼티는 전역 함수 및 전역 변수와 비슷하게 작동하며 드물게 사용되어야 한다. 그것들은 기능이 특정 타입의 스코프에 있거나 Objective-C 상호 운용성이 필요한 경우 유용하다.

### 옵셔널

`nil` 값을 취할 수 있는 곳에 변수 및 함수의 반환형을 `?`을 사용하여 옵셔널로 선언하라.

사용되기 전에 이후에 초기화될 것이라고 알고 있는 인스턴스 변수, 예를 들면 `viewDidLoad()`에서 설정될 서브뷰에 대해서만 `!`을 사용하여 암시적 추출 타입을 사용하라. 대부분의 다른 경우 암시적 추출 옵셔널보다 옵셔널 바인딩을 사용하라.

옵셔널 값에 접근할 때, 그 값에 오직 한 번 접근하거나 체인에 많은 옵셔널이 있을 때 옵셔널 체이닝을 사용하라.

```swift
textContainer?.textLabel?.setNeedsDisplay()
```

한 번 언래핑하고 여러 개의 작업을 수행하는 것을 더 편리하게 할 때 옵셔널 바인딩을 사용하라.

```swift
if let textContainer = textContainer {
  // do many things with textContainer
}
```

옵셔널 변수 및 프로퍼티에 이름을 지을 때, 그것들이 옵셔널이라는 것이 이미 타입 선언에 나타나 있다면 `optionalString`이나 `maybeView`와 같이 네이밍하지 마라.

옵셔널 바인딩에 대하여, `unwrappedView`나 `actualLabel`과 같은 이름을 사용하기보다는, 가능할 때마다 기존 이름을 넌지시 나타내어라.

#### 선호:

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
  // do something with unwrapped subview and volume
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in
  guard let self = self else { return }
  self.alpha = 1.0
}
```

#### 선호하지 않음:

```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    // do something with unwrappedSubview and realVolume
  }
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in
  guard let strongSelf = self else { return }
  strongSelf.alpha = 1.0
}
```

### 지연 초기화

오브젝트의 수명을 세밀하게 제어하기 위해 지연 초기화를 사용하는 것을 고려하라. 이는 특히 뷰를 지연하여 로드하는 `UIViewController`에 대하여 그렇다. 즉시 호출되는 클로저 `{ }()`나 private 팩토리 메소드를 호출할 때 사용할 수 있다.

```swift
lazy var locationManager = makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
}
```

**알아두기**

- `[unowned self]`는 여기서 요구하는 것이 아니다. 리테인 사이클은 생성되지 않는다.
- 로케이션 매니저는 사용자에게 권한을 요청하기 위한 UI를 띄우는 사이드 이펙트를 갖는다. 때문에 여기서는 세밀한 제어가 필요하다.

### 타입 추론

간결한 코드를 산호하고 컴파일러가 하나의 인스턴스의 상수나 변수에 대한 타입을 추론할 수 있게 하라. 타입 추론은 작고, 비어 있지 않은 배열과 딕셔너리에도 적절하다. 필요하다면 `CGFloat`나 `Int16`과 같은 특정 타입을 명시하라.

#### 선호:

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

#### 선호하지 않음:

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names = [String]()
```

#### 빈 배열과 디셔너리에 대한 타입 선언

빈 배열과 딕셔너리에 대하여 타입 선언을 사용하라. (크고, 여러 줄의 리터럴로 할당된 배열과 딕셔너리에 대하여 타입 선언을 사용하라.)

#### 선호:

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

#### 선호하지 않음:

```swift
var names = [String]()
var lookup = [String: Int]()
```

**알아두기** : 이 가이드라인을 따르는 것은 설명적인 이름을 고르는 것이 이전보다 훨씬 더 중요하다는 것을 의미한다.

### Syntatic Sugar

전체 제네릭 문법을 사용하기보다는 타입 선언의 단축 버전을 선호하라.

#### 선호:

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

#### 선호하지 않음:

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## 함수 vs. 메소드

어떠한 클래스나 타입에 붙어 있지 않은 전역 함수는 드물게 사용되어야 한다. 가능하다면 전역 함수 대신 메소드를 사용하라. 이는 가독성과 찾을 수 있는 능력을 도와준다.

전역 함수는 특정 타입이나 인스턴스와 연관되지 않았을 때 가장 적절하다.

#### 선호:

```swift
let sorted = items.mergeSorted()  // easily discoverable
rocket.launch()  // acts on the model
```

#### 선호하지 않음:

```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

#### 전역 함수의 예외:

```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x, y, z)  // another free function that feels natural
```

## 메모리 관리

프로덕션 코드가 아니고, 튜토리얼 데모 코드일지라도, 코드는 참조 사이클을 생성해서는 안된다. 오브젝트 그래프를 분석하고 `weak`와 `unowned` 참조를 사용하여 강한 사이클을 막아라. 대안으로, 사이클을 막기 위해 값 타입 (`struct`, `enum`) 을 사용하라.

### 오브젝트의 수명 연장하기

`[weak self]`와 `guard let self = self else { return }` 상용구를 사용하여 오브젝트의 수명을 연장하라. `[weak self]`는 `self`가 클로저보다 오래 남아 있을 것이라는 것이 즉시 명백하지 않은 곳에서 `[unowned self]`보다 선호된다. 명시적으로 수명을 연장하는 것은 옵셔널 체이닝보다 선호된다.

#### 선호:

```swift
resource.request().onComplete { [weak self] response in
  guard let self = self else {
    return
  }
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

#### 선호하지 않음:

```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

#### 선호하지 않음:

```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## 접근 제어

튜토리얼에서 모든 접근 제어 선언은 주요 토픽과 동떨어져 있으며 필수적이지 않다. 하지만, 적절하게 `private`과 `fileprivate`을 사용하여 명백함을 추가하고 캡슐화를 촉진하라. `fileprivate`보다 `private`을 선호하라. 오직 컴파일러가 주장할 때만 `fileprivate`을 사용하라.

모든 접근 제어 명세를 필요로 할 때만 명시적으로 `open`, `public`, `internal`을 사용하라.

프로퍼티 지정자 앞에 접근 제어를 사용하라. 접근 제어 이전에 오는 유일한 것은 `static` 지정자나 `@IBAction`, `@IBOutlet`, `@discardableResult`와 같은 특성 뿐이다.

#### 선호:

```swift
private let message = "Great Scott!"

class TimeMachine {  
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

#### 선호하지 않음:

```swift
fileprivate let message = "Great Scott!"

class TimeMachine {  
  lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

## 흐름 제어

`while-condition-increment` 스타일보다 `for` 루프의 `for-in` 스타일을 사용하라.

#### 선호:

```swift
for _ in 0..<3 {
  print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
  print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
  print(index)
}

for index in (0...3).reversed() {
  print(index)
}
```

#### 선호하지 않음:

```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}


var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```

### 삼항 연산자

삼항 연산자 `? : `는 이것이 코드의 명백함이나 단정함을 증가시킬 때만 사용되어야 한다. 하나의 조건은 일반적으로 평가되어야 하는 모든 것이다. 여러 개의 조건을 평가하는 것은 일반적으로 `if`문을 사용하거나 인스턴스 변수로 리팩토링하여 더 이해하기 쉬워진다. 일반적으로, 삼항 연산자의 가장 좋은 사용은 변수에 할당 중이고 어떠한 값을 사용할지 결정하는 동안이다.

#### 선호:

```swift
let value = 5
result = value != 0 ? x : y

let isHorizontal = true
result = isHorizontal ? x : y
```

#### 선호하지 않음:

```swift
result = a > b ? x = c > d ? c : d : y
```

## Golden Path

조건절과 함께 코딩할 때, `if`문을 중첩시키지 않는다. 여러 개의 리턴문은 괜찮다. `guard`문이 이를 위해 생긴 것이다.

#### 선호:

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}
```

#### 선호하지 않음:

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies

      return frequencies
    } else {
      throw FFTError.noInputData
    }
  } else {
    throw FFTError.noContext
  }
}
```

여러 개의 옵셔널이 `guard`나 `if let`으로 언래핑될 때, 가능하다면 조합하여 중첩을 최소화하라. 조합된 버전에서, `guard`를 그 자신의 라인에 두고, 각각의 조건을 그 자신의 라인에서 들여쓰기하라. `else`절은 조건들과 일치한 들여쓰기 수준을 갖고, 코드는 추가적인 들여쓰기 수준을 갖는다.

#### 선호:

```swift
guard 
  let number1 = number1,
  let number2 = number2,
  let number3 = number3 
  else {
    fatalError("impossible")
}
// do something with numbers
```

#### 선호하지 않음:

```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```

### 실패하는 Guard문

Guard문은 어떠한 방법으로 탈출할 필요가 있다. 일반적으로, 이는 `return`, `throw`, `break`, `continue`, `fatalError()`와 같은 간단한 한 줄 문장이어야 한다. 큰 코드 블록은 피해야 한다. 정리하는 코드가 여러 개의 탈출 지점에서 필요하다면, 정리 코드의 중복을 피하기 위해 `defer` 블록을 사용하는 것을 고려하라.

## 세미콜론

Swift는 코드에서 각각의 선언문 뒤에 세미콜론을 필요로 하지 않는다. 하나의 라인에 여러 개의 선언문을 조합하고 싶을 때만 필요하다.

세미콜론으로 분리된 여러 개의 선언문을 하나의 라인에 작성하지 마라.

#### 선호:

```swift
let swift = "not a scripting language"
```

#### 선호하지 않음:

```swift
let swift = "not a scripting language";
```

## 괄호

조건절 주위에 있는 괄호는 필수적이지 않으며 생략될 필요가 있다.

#### 선호:

```swift
if name == "Hello" {
  print("World")
}
```

#### 선호하지 않음:

```swift
if (name == "Hello") {
  print("World")
}
```

더 큰 표현식에서, 옵셔널 괄호는 때때로 코드를 더 명백하게 읽을 수 있게 할 수 있다.

#### 선호:

```swift
let playerMark = (player == current ? "X" : "O")
```

## 여러 줄 문자열 리터럴

긴 문자열 리터럴을 만들 때, 여러 줄 문자열 리터럴 문법을 사용하는 것을 장려한다. 할당하는 것과 같은 라인에서 리터럴을 열지만 그 라인에 텍스트를 포함하지는 말아라. 텍스트 블록에 추가적인 들여쓰기 수준을 적용하라.

#### 선호:

```swift
let message = """
  You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

#### 선호하지 않음:

```swift
let message = """You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

#### 선호하지 않음:

```swift
let message = "You cannot charge the flux " +
  "capacitor with a 9V battery.\n" +
  "You must use a super-charger " +
  "which costs 10 credits. You currently " +
  "have \(credits) credits available."
```

## 이모지 사용하지 않기

프로젝트에 이모지를 사용하지 말아라. 실제로 그 코드에 타이핑할 독자들에게, 이는 불필요한 충돌의 근원이 될 수있다. 그것은 귀여울지라도, 그것은 학습에 추가되지 않으며 독자를 위한 코딩 흐름을 방해한다.