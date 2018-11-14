## iOS를 구성하는 것

- Cocoa Touch
  - iOS의 UI 계층
- Media
  - Core Animation, Core Audio 등이 포함된 계층
- **Core Services**
  - Core Location, Networking, File Access 등이 포함된 계층
- Core OS
  - iOS는 Unix 기반 (BSD계열 Unix) : C언어 기반

---

AppDelegate.swift / Assets.xcassets / LaunchScreen.storyboard를 묶어 Supporting Files 폴더에 넣어두었다.

option을 누른 채로 마우스 휠을 사용하여 스토리보드 확대/축소가 가능하다. 듀얼모니터 작업시 유용할듯

스토리보드에서 오브젝트를 코드와 연결시킬 때, action으로 연결시키는 경우 매개변수의 종류(없음 / sender / sender, event), 터치 이벤트 종류 등을 지정할 수 있음

함수 정의시 영어를 읽는 것처럼 정의할 수 있도록 노력하기.

`func flipCard(withEmoji emoji: String, on button: UIButton)`

`flipCard(withEmoji: "", on: sender)` : flip card with emoji on sender

### 이벤트 메소드의 첫 전달인자 레이블이 _인 이유

iOS에서 메세지를 보냄 (Objective-C 관련)

Objective-C에는 내부 이름 (매개 변수 이름), 외부 이름 (전달인자 레이블)이 없어서 그렇다.

전달인자 레이블에 와일드카드를 사용하는 것은 흔히 있는 일은 아니다.

문서에서 Overview를 찬찬히 읽는 것은 시간이 얼마 걸리지 않으나 (영어가 모국어인 사람들 기준..) 전체 내용을 이해하는데 큰 도움을 줄 수 있다. 자주 사용하는 클래스의 Overview는 모두 읽어볼 것!

코드는 복사하지 말아야 한다.

프로퍼티의 변화를 UI에 뿌려주기 위해 프로퍼티 감시자*Property Observer*를 활용할 수 있다.

Outlet Collection으로 스토리보드에서 코드로 연결하면 해당 타입의 배열 형태로 선언되고, 해당 변수에 알맞은 스토리보드 오브젝트를 연결할 수 있다.

변수명 블록지정 -> 마우스 오른쪽 클릭 -> Refactor -> Rename… / 변수명 블록지정 -> command 누른 채 마우스 왼쪽 클릭 -> Rename

`index(of:)` 메소드는 배열에서 매개변수로 들어온 것의 인덱스를 반환한다.. 자동완성에는 표시되지 않는다. 왜?

`Int`와 `Int? (Optional<Int>)`는 전혀 관계 없음. 옵셔널은 열거형으로 구현되어 있음. 값이 있는 경우 (`.some`)의 연관 값으로 `Int` 값이 들어가 있는 것이다.