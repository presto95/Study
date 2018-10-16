### [원문](https://www.raywenderlich.com/49-custom-keyboard-extensions-getting-started)



커스텀 키보드 익스텐션은 당신의 애플리케이션 밖에 키보드를 제공할 수 있는 능력을 제공합니다. 이 튜토리얼에서, 당신은 자동완성과 같은 진보된 특성도 있는 커스텀 키보드 익스텐션을 만들어 볼 것입니다.

---

iOS 8까지, iOS에서 커스텀 키보드를 만들기 위한 당신의 능력은 매우 한정적이었습니다. 당신의 유일한 선택지는 `UITextField` 와 `UITextView` 에 커스텀 인풋 뷰를 만드는 것뿐이었는데, 이것은 오직 당신의 애플리케이션 안에서만 커스텀 키보드를 제공할 수 있음을 의미했습니다.

수평선에서 떠오르는 영웅처럼, 앱 익스텐션 *app extensions* 이 구조를 위해 왔습니다! 이것들은 당신의 애플리케이션 바깥에 컨텐츠를 제공할 수 있는 능력을 가져다줍니다. 키보드 익스텐션은 특히 사용자가 좋아하는 어느 애플리케이션이든 텍스트를 입력하는 데 커스텀 인터페이스를 제공하게 해줍니다.

이 튜토리얼에서, 당신은 다른 애플리케이션에서 사용될 수 있는 커스텀 키보드 익스텐션을 만들어 볼 것입니다.

#### 키보드 익스텐션 개념

커스텀 키보드 익스텐션은 사용자들이 사용가능한 키보드 리스트에 추가적으로 키보드를 추가하게 해줍니다. 부가적인 키보드의 좋은 예시는 iOS에 이미 들어가 있는 이모지 키보드입니다. 당신이 이 키보드를 활성화한다면, 기본 시스템 키보드의 왼쪽 아래 모서리에 위치한 지구본을 탭하여 이모지 키보드로 전환할 수 있습니다.

커스텀 키보드는 완전하게 시스템 키보드를 대체할 수 있고, 또는 이모지 키보드처럼 보충의 역할을 할 수 있습니다.

당신의 커스텀 키보드 익스텐션은 버튼 탭과 같은 유저 인터랙션을 다루는 UI를 제공합니다. 그것들은 이 액션들을 `Strings` 으로 변환하고 호스트 텍스트 인풋 뷰에 전달합니다. 이것은 [iOS가 지원하는 유니코드 문자](https://unicode.org/emoji/charts/full-emoji-list.html) 어느 것이든 사용할 수 있는 자유를 제공합니다.

하지만 커스텀 키보드를 추가한 것은 그것이 항상 사용가능하다는 것을 의미하지는 않습니다. 커스텀 키보드가 금지되는 몇 가지 예시들이 있습니다.

- `secureTextEntry` 가 `true` 로 설정된 보안 텍스트 인풋 필드
- 캐리어 문자 제한으로 인한 `UIKeyboardTypePhonePad` 와 `UIKeyboardTypeNamePhonePad` 와 같은 키보드 타입을 갖는 텍스트 입력
- 개발자가 `AppDelegate` 의 `application(_:shouldAllowExtensionPointIdentifier:)` 메소드를 통해 키보드 익스텐션의 사용성을 감소시킴

#### 사용자의 기대에 부응하기

키보드 제작자로서 당신의 성공은 사용자들의 높은 기대를 충족시킬 수 있는 당신의 능력에 좌우됩니다. 키보드에서 오류는 용납되지 않습니다. 대부분의 사용자들은 그들의 애플 기기로부터 특정 기능을 기대하는 것을 학습했고, 키보드 디자인의 미묘함을 이해하는 가장 좋은 방법은 시스템 키보드로 연구하고 놀아보는 것입니다.

당신은 기본 iOS 키보드가 즉각 반응하고, 깔끔하고, 텍스트 입력의 종류에 따라 동적으로 응답하고, 자동완성과 텍스트 대치를 제공하고, **잘 동작**한다는 것을 알고 있을 것입니다.

이것들은 단지 당신의 사용자들이 가질 기대 중 몇몇 것에 불과합니다. 당신의 작업은 이러한 기대를 충족시키거나 그 이상을 해내는 것입니다. **키보드는 사용자가 하려는 것을 절대 방해해서는 안되며**, 대부분의 사용자 경험을 바로 해결해야 한다는 것을 기억하십시오.

#### 요구사항

기본적은 iOS 애플리케이션처럼, 애플이 승인하기 전에 키보드가 충족시켜야 하는 특정 요구 사항이 있습니다.

- **다음 키보드 버튼** : 모든 키보드는 사용자가 활성화한 키보드 리스트에서 다음 키보드로 전환할 수 있는 방법을 필요로 합니다. 시스템 키보드에서, 지구본 키가 이 액션을 수행합니다. 일관성을 위해, 시스템 키보드가 그랬던 것처럼, 이 키를 좌측 하단 모서리에 위치시키는 것을 추천합니다.
- **애플리케이션의 목적** : 모든 키보드 익스텐션은 호스트 애플리케이션에서 번들로 제공되어야 합니다. 이 애플리케이션은 실제로 목적을 제공하고 사용자에게 받아들일 수 있는 기능을 제공해야 합니다. 맞습니다. 이것은 주관적입니다. 또한 맞습니다. 그것은 당신이 정말로 당신의 호스트 애플리케이션에 시간과 생각을 들여야 할 것이라는 말입니다!
- **신뢰** : 데이터 유출에 관하여 지속적으로 나오고 있는 뉴스는 사용자들이 그들의 데이터에 대하여 극도로 민감해지게 합니다. 당신의 애플리케이션에서 데이터가 흐르는 주요 지점 중 하나로서, 키보드는 사용자 데이터에 대해 잠재적으로 취약한 지점입니다. 사용자가 타이핑하는 데이터의 안전을 보장하는 것은 키보드 개발자의 책임입니다. 무심코 키 로거를 만들지 마십시오!

이러한 모든 배경 지식으로, 당신은 손을 더럽게 하고 싶어서 안달이 나있을 것입니다.

#### 시작하기

이 튜토리얼에서, 당신은 *모스 부호* 키보드를 만들어볼 것입니다. 모스 부호에 유창하지 않아도 걱정하지 마십시오. 이 튜토리얼이 끝나면 유창해질 것이기 때문입니다!

시나리오는 다음과 같습니다. 당신은 사람들에게 모스 부호를 가르치기 위한 멋진 애플리케이션을 만들었습니다. 하지만 그들은 오직 당신의 애플리케이션을 사용하는 동안만 연습할 수 있습니다. 이제 키보드 익스텐션이란 것이 존재함을 배웠고, 당신의 애플리케이션 바깥에서도 같은 굉장한 기능을 제공하기로 마음먹었습니다!

이 프로젝트를 위한 재료를 다운로드받고 Xcode에서 스타터 프로젝트를 여십시오. 이 튜토리얼의 맨 위 또는 맨 아래에서 링크를 찾을 수 있습니다. 이것은 키보드 익스텐션을 제공하기 위해 사용될 호스트 애플리케이션입니다.

스타터 애플리케이션을 빌드하고 실행하십시오. 모스 부호 트레이너 화면을 확인할 수 있을 것입니다.

이 애플리케이션은 사용자가 커스텀 키보드와 치트 시트를 사용하여 모스 부호를 연습할 수 있게 합니다.

모스 부호는 문자를 나타내기 위해 일련의 *점* 과 *선* 신호를 사용합니다. 이 구현은 현재 일련의 신호, 그리고 그것이 나타내는 숫자나 문자를 키보드 위의 회색 막대 내에 표시합니다.

점과 선의 조합을 탭하고 문자를 만들어 보십시오. 문자를 완성하고 다음으로 넘어가기 위해 스페이스 키를 누르십시오. 실제 스페이스를 입력하기 위해 스페이스 키를 두 번 누르십시오.

당신이 키보드의 기능에 적응한 후, 이렇게 생각할 지 모릅니다. "내 작업을 끝났어!" 그것은 멋진 튜토리얼이지만, 불운하게도 당신은 이 키보드를 샘플 애플리케이션에서만 사용할 수 있으며, 그것은 커스텀 키보드의 모든 목적에 반합니다!

이미 호스트 애플리케이션에 제공된 것들, 그리고 키보드 익스텐션에서 해야 할 남은 것들을 잘 이해하기 위해, 프로젝트 내비게이터에서 폴더들을 확장하고 둘러보십시오.

파일들이 무슨 일을 하는지에 대한 요약은 다음과 같습니다.

- `PracticeViewController.swift` : 데모 애플리케이션의 UI와 기능을 설정합니다.
- `MorseColor.swift` : 키보드는 밝고 어두운 디스플레이 모두를 위한 색상 조합을 지원해야 합니다. 이 파일은 각각의 타입에 대한 색상 조합을 정의합니다.
- `MorseData.swift` : 일련의 점과 선을 이에 상응하는 문자로, 또는 그 정반대로 변환하는 방법을 제공합니다.
- `KeyboardButton.swift` : 키보드 키의 생김새와 느낌을 커스터마이징한 `UIButton` 의 서브클래스입니다.
- `MorseKeyboardView.swift` / `MorseKeyboardView.xib` : 모스 키보드의 레이아웃과 기능을 다루는 커스텀 `UIView` 입니다. 여기에 모든 모스 부호에 특정한 로직이 위치해 있습니다. 그것은 `MorseKeyboradViewDelegate` 프로토콜을 선언하여 텍스트 인풋 뷰에서의 특정 액션을 수행하기 위해 위임자에게 알려줍니다. 이것은 키보드 익스텐션 뿐만 아니라 키보드 인풋 뷰에서도 `MorseKeyboardView` 를 사용할 수 있게 할 것입니다.

이제 당신은 포함된 애플리케이션에 익숙해져 있고, 키보드 익스텐션을 만들 시간이 되었습니다!

#### 키보드 익스텐션 생성하기

존재하는 프로젝트에 키보드 익스텐션을 생성하기 위해, *File > New > Target...* 에서 *iOS* 를 선택하고 *Custom Keyboard Extension* 을 선택하십시오. *Next* 를 클릭하여 당신의 키보드를 *MorseKeyboard* 라고 작명하십시오. *Embedded in Application* 에 *MorseCoder* 를 선택해야 합니다.

*Finish* 를 눌러 익스텐션을 생성하십시오. 스키마 활성을 위한 팝업이 뜬다면, *Activate* 를 선택하십시오.

Xcode는 프로젝트 내비게이터에 익스텐션을 위한 새로운 폴더를 생성합니다. *MorseKeyboard* 를 확장하여 포함하는 컨텐츠들을 드러내십시오.

생성된 키보드 익스텐션 파일 각각을 살펴봅시다.

- `KeryboardViewController.swift` : 커스텀 키보드 익스텐션을 위한 주요한 뷰 컨트롤러의 역할을 할 `UIInputViewController` 의 서브클래스입니다. 여기에 `MorseKeyboardView` 를 연결하고 `PracticeViewController` 에서 된 것과 비슷하게 키보드를 위한 커스텀 로직을 구현할 것입니다.
- `Info.plist` : 익스텐션을 위한 메타데이터를 정의할 `plist` 입니다. `NSExtension` 아이템은 키보드의 특정 설정을 포함합니다. 이 아이템에 대한 중요성은 다음에 다룰 것입니다.

이게 끝입니다. 두 개의 파일이 키보드 익스텐션을 사용하기 위한 전부입니다!

`KeyboardViewController.swift` 파일을 열어 생성된 템플릿을 자세하게 들여다 보십시오. `viewDidLoad()` 안에 있는 많은 코드를 확인할 수 있을 것입니다. 일찍히 언급했듯이, 커스텀 키보드의 주요 요구사항은 다른 키보드로 전환할 수 있는 키를 제공하는 것입니다.

이 코드는 프로그래밍적으로 `handleInputModeList(from:with:)` 액션을 포함하는 `UIButton` 을 키보드에 추가합니다. 이 메소드는 두 개의 다른 기능을 제공합니다.

- 버튼을 눌러 사용자가 활성화한 리스트에 있는 다음 키보드로 키보드를 전환합니다.
- 버튼을 길게 눌러 키보드 리스트를 보여줍니다.

키보드를 실행하여 테스트해볼 시간입니다! 불운하게도, 키보드 익스텐션을 실행하고 디버깅하는 것은 전형적인 애플리케이션보다 약간 더 복잡합니다.

#### 커스텀 키보드 활성화하기

Xcode 상단의 스키마 셀렉터에서 *MorseKeyboard* 스키마를 선택하십시오.

스키마를 빌드하고 실행하면, 그것이 포함될 애플리케이션을 선택할 수 있을 것입니다. 당신의 목적을 위해, 사용 가능한 텍스트 필드를 가지고 있는 *Safari* 애플리케이션을 선택하고, *Run* 을 클릭하십시오.

Safari가 로드된 후, 애플리케이션을 최소화하십시오. 설정 앱에서 *일반 > 키보드 > 키보드* 를 선택하고 *새로운 키보드 추가...* 를 선택하십시오. 마지막으로, *MorseCoder* 를 선택하십시오.

운좋게도, 당신은 오직 각각의 디바이스에 단 한번만 키보드를 추가하면 됩니다. 설정 앱을 닫고 *Safari* 앱을 실행하십시오. 시스템 키보드를 나타내기 위해 주소 바를 선택하십시오.

일단 당신이 시스템 키보드를 확인했다면, 지구본 키를 길게 누르십시오. 새로 추가된 *MorseKeyboard* 를 선택하십시오.

그러면 당신의 매우 밋밋한 키보드가 표시될 것입니다! *다음 키보드* 버튼을 탭하거나 길게 누르면 시스템 키보드로 되돌릴 수 있습니다.

이것이 기본 기능의 전부입니다. 이제는 커스텀 UI로 키보드 빅 리그에 들어갈 시간입니다!

#### 키보드 UI 커스터마이징

컨테이너 애플리케이션과 익스텐션 모두에서 UI를 다시 만들기보다는, 모든 타겟에서 같은 코드를 공유할 수 있습니다.

프로젝트 내비게이터에서, 아래의 여섯 개의 파일을 *Command-클릭* 하여 선택하십시오.

- `MorseColor.swift`
- `MorseData.swift`
- `KeyboardButton.swift`
- `MorseKeyboardView.swift`
- `MorseKeyboardView.xib`
- `Assets.xcassets`

이제 파일 인스펙터를 열고 선택된 파일들을 *MorseKeyboard* 타겟에 추가하십시오.

이렇게 하면 독립적인 MorseCoder 애플리케이션과 MorseKeyboard 앱 익스텐션, 두 가지 컴파일 타겟에 이 파일들이 추가됩니다. 이것은 아마 코드를 공유하는 가장 간단한 방법일 것입니다. 정확하게 같은 소스 코드 파일을 두 번 사용하는데, 다른 빌드 프로덕트를 생성하는 다른 타겟을 위한 것입니다.

이제 당신의 키보드 익스텐션은 이러한 파일에 접근할 수 있고, 같은 UI를 재사용할 수 있습니다. `KeyboardViewController.swift` 파일을 열고 `KeyboardViewController` 에 있는 모든 컨텐츠들을 다음으로 교체하십시오.

```swift
// 1
var morseKeyboardView: MorseKeyboardView!

override func viewDidLoad() {
  super.viewDidLoad()
  
  // 2
  let nib = UINib(nibName: "MorseKeyboardView", bundle: nil)
  let objects = nib.instantiate(withOwner: nil, options: nil)
  morseKeyboardView = objects.first as! MorseKeyboardView
  guard let inputView = inputView else { return }
  inputView.addSubview(morseKeyboardView)
  
  // 3
  morseKeyboardView.translatesAutoresizingMaskIntoConstraints = false
  NSLayoutConstraint.activate([
    morseKeyboardView.leftAnchor.constraint(equalTo: inputView.leftAnchor),
    morseKeyboardView.topAnchor.constraint(equalTo: inputView.topAnchor),
    morseKeyboardView.rightAnchor.constraint(equalTo: inputView.rightAnchor),
    morseKeyboardView.bottomAnchor.constraint(equalTo: inputView.bottomAnchor)
    ])
}
```

`KeyboardViewController` 는 이제 키보드 뷰를 설정하는 코드를 담고 있습니다.

1. `MoreKeyboardView` 객체 참조를 담는 프로퍼티
2. `MorseKeyboardView` 의 인스턴스를 컨트롤러의 루트 `inputView` 에 추가한다.
3. 슈퍼뷰에 `morseKeyboardView` 를 고정시키는 제약들이 추가되고 활성화된다.

처음부터 UI를 만들기보다는, 간단하게 호스트 애플리케이션에서 같은 코드를 재사용하기만 하면 됩니다. Safari를 통해 키보드 익스텐션을 빌드하고 실행하십시오. 주소 바를 선택하고 지구본 키를 길게 눌러 MorseKeyboard를 선택하십시오.

조금 더 나아진 것 같습니다. 키들을 클릭할 수 있고, 올바른 문자가 키보드 프리뷰에 보여지지만, 주소 바는 갱신되고 있지 않습니다.

> 키보드의 높이가 시스템 키보드의 그것과 다르다는 것을 알아차렸을 것입니다. 키보드 익스텐션은 자동으로 nib의 오토레이아웃 제약에 기반하여 높이를 추론합니다.

좋아 보이지만, 무엇인가가 빠져 있습니다. 다음 키보드 버튼은 필수입니다!

`MorseKeyboardView.xib` 파일을 열어 이미 nib에 지구본 키가 추가되어 있음을 확인하십시오.

음...무엇인가가 다음 키보드 키를 가리고 있는 것 같습니다. `MorseKeyboardView.swift` 를 열어 `setKeyboardVisible(_:)` 메소드를 확인하십시오.

이것은 다음 키보드 키를 숨기거나 보여주기 위해 특정 제약을 활성화하고 비활성화하는 사용자 정의 메소드입니다. 그것은 당신이 그 키를 숨길 필요가 있는 상황이 있기 때문에 존재합니다.

`UIInputViewController` 는 `needsInputModeSwitchKey` 프로퍼티를 정의하여 당신의 커스텀 키보드가 다음 키보드 키를 보여주는 것이 필수적인지 아닌지 알려줍니다. 그것이 `false` 가 되는 예는 당신의 키보드가 iPhone X에서 실행되고 있을 때입니다. 이 디바이스는 다음 키보드 키를 기본적으로 올라온 키보드의 아래에서 제공하므로, 커스텀 키보드에 그 키를 추가할 필요가 없습니다.

당신은 이 프로퍼티를 지구본 키의 가시성을 조정하기 위해 사용할 것입니다. `KeyboardViewController.swift` 파일을 열어 `viewDidLoad()` 아래에 다음의 줄을 추가하십시오.

```swift
morseKeyboardView.setNextKeyboardVisible(needsInputModeSwitchKey)
```

이것은 모스 키보드에게 `needsInputModeSwitchKey` 의 값에 따라 다음 키보드 키를 숨길지 숨기지 않을지에 대해 알려줍니다.

이것을 테스트하기 위해, 여러 개의 디바이스에서 키보드를 실행할 필요가 있습니다. iPhone X와 다른 iPhone에서 빌드하고 실행하십시오. 새로운 디바이스를 실행하고 있다면 *설정 > 일반 > 키보드 > 키보드 > 새로운 키보드 추가...* 에서 키보드를 추가해야 함을 기억하십시오.

당신은 iPhone X에는 이미 시스템 지구본 키가 있기 때문에 커스텀 키보드에 다음 키보드 키를 숨겼음을 확인해야 합니다. 또한 다른 디바이스에서는 기대한 대로 커스텀 키를 확인할 수 있을 것입니다.

지금 당신의 키는 어떤 동작을 하지 않습니다. 하지만 솔직히 말해서 다른 것도 그렇습니다. 그것을 고칠 시간입니다!

#### 키보드 액션 붙이기

템플릿처럼, 당신은 지구본 키에 프로그래밍적으로 액션을 추가할 것입니다. `viewDidLoad()` 의 마지막 부분에 다음의 코드를 추가하십시오.

```swift
morseKeyboardView.nextKeyboardButton.addTarget(self, action: #selector(handleInputModeList(from:with:)), for: .allTouchEvents)
```

이것은 자동으로 키보드 전환흘 다루는 `handleInputModeList` 메소드를 다음 키보드 키의 액션으로 추가합니다.

`MorseKeyboardView` 의 키들에 남은 것은 이미 그 뷰 안에 있는 액션들에 붙여져 있습니다. 현재 그것들이 동작하지 않는 이유는 이 이벤트들을 받을 `MorseKeyboardViewDelegate` 를 구현하지 않았기 때문입니다.

`KeyboardViewController.swift` 파일의 하단에 다음의 익스텐션 코드를 추가하십시오.

```swift
// MARK: - MorseKeyboardViewDelegate
extension KeyboardViewController: MorseKeyboardViewDelegate {
    func insertCharacter(_ newCharacter: String) {

    }

    func deleteCharacterBeforeCursor() {

    }

    func characterBeforeCursor() -> String? {
        return nil
    }
}
```

`MorseKeyboradView` 는 모스 부호와 관련된 모든 로직을 다루고, 사용자가 키를 탭한 후 이러한 메소드들의 다른 조합들을 호출할 것입니다. 당신은 한 번에 하나씩 이 메소드 토막들을 구현할 것입니다.

`insertCharacter(_:)` 는 현재 커서 인덱스 뒤에 새로운 문자를 추가할 시간이라는 것을 알려줍니다. `PracticeViewController.swift` 에서, 당신은 `UITextField` 에 `textField.insertText(newCharacter)` 코드를 사용하여 직접적으로 문자를 추가합니다.

여기에서의 차이점은 커스텀 키보드 익스텐션은 텍스트 입력 뷰에 대한 직접 접근이 되지 않는다는 것입니다. 당신이 접근할 수 있는 것은 `UITextDocumentProxy` 타입의 프로퍼티입니다.

`UITextDocumentProxy` 는 객체에 대한 직접 접근 없이 현재 삽입 지점 주위의 텍스트 문맥을 제공합니다. 이것이 프록시(대리, 위임)인 이유입니다.

이것이 작동하는 방법을 보기 위해, `insertCharacter(_:)` 에 다음의 코드를 추가하십시오.

```swift
textDocumentProxy.insertText(newCharacter)
```

이것은 프록시에게 당신이 새로운 문자를 삽입했다는 것을 알려줍니다. 동작하는 것을 확인하기 위해, 키보드 뷰의 델리게이트를 설정할 필요가 있습니다. 다음의 코드를 `viewDidLoad()` 의 `morseKeyboardView` 프로퍼티를 할당하는 코드 다음에 추가하십시오.

```swift
morseKeyboardView.delegate = self
```

Safari에서 키보드 익스텐션을 빌드하고 실행하여 이 새로운 기능을 테스트하십시오. 다른 키들을 누르고 주소 바에서의 횡설수설한 결과를 확인하십시오.

이것은 우리가 기대했던 것과 정확하게 같진 않지만, 다른 두 개의 `MorseKeyboardViewDelegate` 메소드를 구현하지 않았기 때문일 것입니다.

`deleteCharactersBeforeCursor()` 에 다음의 코드를 추가하십시오.

```swift
textDocumentProxy.deleteBackward()
```

삽입 때와 마찬가지로, 프록시 객체에게 커서 앞에 있는 문자를 지우라고 알려주기만 하면 됩니다.

델리게이트 구현을 마무리하기 위해, `characterBeforeCursor()` 의 컨텐츠를 다음의 코드로 교체하십시오.

```swift
// 1
guard let character = textDocumentProxy.documentContextBeforeInput?.last else {
  return nil
}
// 2
return String(character)
```

이 메소드는 `MorseKeyboardView` 에게 커서 앞에 있는 문자가 무엇인지 알려줍니다. 여기 키보드 익스텐션에서 이것을 달성할 수 있는 방법이 있습니다.

1. `textDocumentProxy` 는 `documentContextBeforeInput` 프로퍼티를 노출하는데, 이것은 커서 앞에 있는 전체 문자열을 포함합니다. 모스 키보드에서는 오직 마지막 문자를 필요로 합니다.
2. `String` 으로 `Character` 를 반환합니다.

MorseKeyboard 스키마를 빌드하고 실행하여 Safari에 붙이십시오. 당신의 커스텀 키보드로 전환한 후 써보십시오. 당신이 입력한 패턴에 맞는 문자가 주소 바에 보여지고 있음을 확인할 수 있습니다!

여기까지 해서 간접적으로 텍스트 인풋 뷰와 상호작용하고 있지만, 역방향으로 상호작용하는 것은 어떻습니까?

#### 입력 이벤트에 응답하기

`UIInputViewController` 가 `UITextInputDelegate` 를 구현하고 있기 때문에, 특정 이벤트가 텍스트 인풋 뷰에서 발생할 때 업데이트를 받습니다.

> 불운하게도, 이 기능이 `UIKit` 에 추가된 이후 수 년이 흘렀음에도 불구하고, 모든 메소드들이 문서화된 대로 작동하는 것은 아닙니다.
>
> Caveat coder.

당신은 `textDidChange(_:)` 델리게이트 메소드에 초점을 맞출 것이고, 이것을 어떻게 키보드 익스텐션에서 사용할 수 있겠습니까? 이름이 당신을 속일 수 있게 하지 마십시오. **이 메소드는 텍스트가 변화할 때 호출되지 않습니다.** 그것은 키보드가 보여지거나 숨겨진 후, 그리고 커서 위치나 선택이 변화한 후에 호출됩니다. 텍스트 인풋 뷰의 생김새에 기반하여 키보드의 색상 조합을 적용할 최고의 위치입니다.

`KeyboardViewController` 에서, `viewDidLoad()` 하단에 다음의 메소드 구현을 추가하십시오.

```swift
override func textDidChange(_ textInput: UITextInput?) {
  // 1
  let colorScheme: MorseColorScheme

  // 2
  if textDocumentProxy.keyboardAppearance == .dark {
    colorScheme = .dark
  } else {
    colorScheme = .light
  }

  // 3
  morseKeyboardView.setColorScheme(colorScheme)
}
```

이 코드는 텍스트 인풋 뷰의 생김새를 확인하고, 다음에 따라 키보드의 생김새를 적용합니다.

1. `MorseColorScheme` 은 `MorseColor.swift` 에 정의된 사용자 정의 열거형 타입입니다. 그것은 `dark` 그리고 `light` 색상 조합을 정의합니다.
2. 어떤 색상 조합을 사용할지 결정하기 위해, `textDocumentProxy` 의 `keyboardAppearance` 프로퍼티를 확인합니다. 이것은 또한 `dark` 와 `light` 선택지를 제공합니다.
3. 색상 조합을 갱신하기 위해 모스 키보드의 `setColorScheme(_:)` 에 결정된 `colorScheme` 을 보냅니다.

테스트하기 위해, 다크 모드에서의 텍스트 인풋에서 키보드를 열 필요가 있습니다. Safari에서 익스텐션을 빌드하고 실행하십시오. 앱을 최소화하고 검색 바를 보여지게 하기 위해 아래로 스와이프 하십시오.

기본 키보드가 이제 어두워졌음을 확인하십시오. 모스 키보드로 전환하십시오.

와! 당신의 커스텀 키보드는 이제 그것이 보여지는 텍스트 인풋 뷰에 적응할 수 있습니다! 당신의 키보드는 정말 좋아졌습니다... 하지만 거기서 왜 멈추는 것인가요?

#### 자동 수정 및 제안

시스템 키보드와 비슷하게 자동 수정과 제안을 추가하는 것은 키보드의 일반적인 기대를 충족시킵니다. 그러나 당신이 이것을 한다면, 당신 자신만의 단어 조합과 로직을 제공할 필요가 있을 것입니다. iOS는 자동으로 이것을 당신에게 제공하지 않습니다.

그러나, 다음의 디바이스 데이터에 접근하여 방법을 제공합니다.

- 사용자의 연락처에서 연결되지 않은 성씨와 이름
- *설정 > 일반 > 키보드 > 텍스트 대치* 에 정의된 텍스트 줄임말
- 매우 제한적인 일반 단어 사전

iOS는 이것을 `UILexicon` 클래스를 통해 수행합니다. 당신은 모스 키보드에서 자동 텍스트 대치를 구현함으로서 이것이 동작하는 방법을 배울 것입니다.

`KeyboardViewController` 에서, `morseKeyboradView` 선언 아래에 다음의 코드를 추가하십시오.

```swift
var userLexicon: UILexicon?
```

이것은 사용자가 입력한 것과 비교하기 위한 단어의 소스로서 디바이스의 어휘집 데이터를 갖고 있을 것입니다.

이 데이터를 요청하기 위해, `viewDidLoad()` 끝에 다음의 코드를 추가하십시오.

```swift
requestSupplementaryLexicon { lexicon in
  self.userLexicon = lexicon
}
```

이 메소드는 디바이스에서 보조 어휘집을 요청합니다. 완료되면 방금 정의한 `userLexicon` 프로퍼티에 저장할 `UILexicon` 객체를 얻게 됩니다.

현재 단어가 어휘집 데이터의 것과 일치하는지 알기 위해, 현재 단어가 무엇인지 알 필요가 있습니다.

`userLexicon` 아래에 다음의 연산 프로퍼티를 추가하십시오.

```swift
var currentWord: String? {
  var lastWord: String?
  // 1
  if let stringBeforeCursor = textDocumentProxy.documentContextBeforeInput {
    // 2
    stringBeforeCursor.enumerateSubstrings(in: stringBeforeCursor.startIndex..., options: .byWords) { word, _, _, _ in
      // 3
      if let word = word {
        lastWord = word
      }
    }
  }
  return lastWord
}
```

어떻게 이 코드가 커서 위치에 기반하여 현재 단어를 얻는지 분석해 봅시다.

1. 커서 앞에 있는 텍스트를 얻기 위해 다시 `documentContextBeforeInput` 을 사용합니다.
2. `enumerateSubstrings` 를 사용하여 문자열에 있는 각 단어를 열거합니다.
3. `word` 를 추출하고 `lastWord` 에 저장합니다. 열거가 끝나면, `lastWord` 가 어떤 것을 가지고 있든 커서 앞의 마지막 단어가 될 것입니다.

이제 당신은 현재 타이핑한 단어를 가지고 있고, 자동 수정 또는 대치를 행할 위치가 필요할 것입니다. 이것을 위한 가장 일반적인 위치는 스페이스 키를 누른 후입니다.

파일의 끝에 다음의 익스텐션을 추가하십시오.

```swift
// MARK: - Private methods
private extension KeyboardViewController {
  func attemptToReplaceCurrentWord() {
    // 1
    guard let entries = userLexicon?.entries, let currentWord = currentWord?.lowercased() else { return }
      
    // 2
    let replacementEntries = entries.filter {
      $0.userInput.lowercased() == currentWord
    }

    if let replacement = replacementEntries.first {
      // 3
      for _ in 0..<currentWord.count {
        textDocumentProxy.deleteBackward()
      }

      // 4
      textDocumentProxy.insertText(replacement.documentText)
    }
  }
}
```

이것은 좋은 코드 조각이므로 단계별로 살펴봅시다.

1. 사용자의 어휘와 현재 단어가 존재하는지 진행 전에 확실하게 합니다.
2. `userInput` 을 현재 단어와 비교하여 어휘 데이터를 필터링합니다. 이 프로퍼티는 대치될 단어를 나타냅니다. 이것의 예시는 *omw* 가 *On my way!* 로 대체되는 것입니다.
3. 일치하는 것을 찾았다면, 텍스트 인풋 뷰에서 현재 단어를 지웁니다.
4. 어휘 프로퍼티 `documentText` 를 사용하여 정의된 대체 텍스트를 삽입합니다.

끝입니다! 이 메소드를 스페이스를 입력한 후에 호출하기 위해, `insertCharacter(_:)` 의 상단 `insertText(_:)` 호출 전에 다음의 코드를 추가합니다.

```swift
if newCharacter == " " {
  attemptToReplaceCurrentWord()
}
```

이 코드는 오직 리터럴 스페이스 문자가 입력된 후에 텍스트 대치를 수행하게 합니다. 이것은 새로운 문자를 시작하기 위해 스페이스 키를 누르는 동안 텍스트를 대치하는 것을 피합니다.

이 새로운 기능을 사용해 보십시오! 

**치트 시트** : *– – – space – – space • – – space space*

"omw" 글자가 "On my way!" 로 대치된 것을 확인했습니다. 이것은 iOS가 테스트 대치로서 자동으로 이것을 추가했기 때문입니다. *설정 > 일반 > 키보드 > 텍스트 대치* 에서 더 많은 단어를 더하여 이 기능을 가지고 놀 수도 있고, 당신의 핸드폰에서 실행하여 연락처에서 이름을 타이핑하여 가지고 놀 수도 있습니다.

이것이 오직 단어의 제한된 범위에서만 동작할지라도, 사용자가 시스템 키보드로부터 기대하기로 학습한 것처럼, 커스텀 키보드가 사용자에게 이 기능을 제공하는 것은 필수적입니다.

이것으로 키보드 익스텐션의 기본 그리고 중간 기능을 정리했습니다... 하지만 더 원한다면 어떤 것이 있겠습니까?

#### 전체 접근 요청하기

키보드 익스텐션에서 더 진보된 것들을 하기 위해, 사용자로부터 그러한 특권을 요청할 필요가 있을 것입니다. **전체 접근**은 사용자가 허용하거나 허용하지 않는 것을 선택할 수 있는 권한입니다. 그것은 당신의 익스텐션에 다음을 포함하는 몇 가지 능력을 가져다 줍니다.

- 위치 정보 서비스 / 이름, 장소, 전화번호를 포함하는 연락처
- 키보드와 호스트 애플리케이션은 iCloud와 인앱 구매 같은 기능을 허용하는 공유 컨테이너를 이용할 수 있습니다.
- 웹 서비스에 접속하기 위한 네트워크 접근
- 호스트 애플리케이션을 통해 키보드의 사용자 정의 자동 수정 어휘를 편집하는 능력

하지만 *Uncle Ben* 이 말했듯, "큰 힘에는 큰 책임이 따릅니다". 민감한 사용자 데이터를 안전하게 다루는 것은 당신에게 달려있습니다.

이 힘을 맛보기 위해, 당신은 키보드 익스텐션으로부터 사용자의 위치에 접근하는 것을 요청할 것입니다. 왜 키보드가 사용자의 위치를 필요로 하는가? 음, 모스 코드는 조난 중에 SOS 메세지와 가장 일반적으로 연관됩니다. "SOS"를 타이핑한 후에 현재 위치를 자동으로 삽입하는 것은 좋을 수 있습니다!

당신이 필요한 첫 번째 것은 사용자의 위치일 것입니다. 이것을 위해 `Core Location` 프레임워크를 사용할 것입니다. `KeyboardViewController.swift` 파일의 상단에 다음의 import문을 추가하십시오.

```swift
import CoreLocation
```

다음으로, `currentWord` 아래에 현재 사용자 위치를 저장하기 위한 프로퍼티를 추가하십시오.

```swift
var currentLocation: CLLocation?
```

이 프로퍼티를 설정하기 위해, `CLLocationManagerDelegate` 프로토콜을 사용할 것입니다. 파일의 하단에 다음의 익스텐션을 추가하십시오.

```swift
// MARK: - CLLocationManagerDelegate
extension KeyboardViewController: CLLocationManagerDelegate {
  func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
    currentLocation = locations.first
  }
}
```

사용자의 위치의 갱신은 이 메소드의 호출을 촉발합니다. 위치 갱신을 시작하기 위해 `CLLocationManager` 인스턴스를 만들 필요가 있습니다. 다음의 프로퍼티를 `currentLocation` 아래에 추가하십시오.

```swift
let locationManager = CLLocationManager()
```

`locationManager` 가 위치 갱신을 시작하기 위해, 몇 가지 프로퍼티를 설정해주어야 할 것입니다. `viewDidLoad()` 의 마지막 부분에 다음의 코드를 추가하십시오.

```swift
locationManager.requestWhenInUseAuthorization()
locationManager.delegate = self
locationManager.desiredAccuracy = kCLLocationAccuracyBest
locationManager.distanceFilter = 100
locationManager.startUpdatingLocation()
```

이것은 위치 매니저 객체에 오직 키보드가 사용중일 때 위치를 갱신하라고 알려줍니다.

> 위치 데이터에 접근하기 위해 `CoreLocation` 을 사용하는 것과 관련하여 [애플 문서](https://developer.apple.com/documentation/corelocation/cllocationmanager) 를 참고하십시오.

마지막 할 것은 "SOS" 메시지를 타이핑한 후에 현재 위치를 삽입하는 것입니다. `insertCharacter(_:)` 의 현재 정의를 다음의 새로운 정의로 교체하십시오.

```swift
func insertCharacter(_ newCharacter: String) {
  if newCharacter == " " {
    // 1
    if currentWord?.lowercased() == "sos", let currentLocation = currentLocation {
      // 2
      let lat = currentLocation.coordinate.latitude
      let lng = currentLocation.coordinate.longitude
      textDocumentProxy.insertText(" (\(lat), \(lng))")
    } else {
      // 3
      attemptToReplaceCurrentWord()
    }
  }

  textDocumentProxy.insertText(newCharacter)
}
```

이것은 다음과 같이 동작합니다.

1. 현재 단어가 "sos" 이고 사용자의 위치를 가지고 있는지 확인합니다.
2. 텍스트 인풋 뷰에 사용자의 현재 위도와 경도를 삽입합니다.
3. 현재 단어가 "sos" 가 아니거나, 위치 정보를 가지고 있지 않다면, 이전에 했던 것처럼 현재 단어를 대치하려는 시도를 합니다.

이제 올드스쿨 조난 신호가 현대 기술 메커니즘과 잘 맞는지 보아야 할 때입니다. 키보드 익스텐션을 Safari에서 빌드하고 실행하여 "sos" 를 타이핑합니다.

**치트 시트** : *• • • space – – – space • • • space space*

음... 아무 일도 일어나지 않습니다. 실망입니다. 콘솔을 확인하면 다음과 같은 것을 확인할 수 있을 것입니다.

물론입니다! 당신은 키보드의 프로퍼티 리스트에서 몇 가지 프라이버시 설정을 빠드렸습니다.

`MorseKeyboard` 폴더 안에 있는 `Info.plist` 파일을 여십시오. `Bundle Version` 을 선택하고 새로운 키-값 쌍을 추가하기 위해 + 기호를 클릭하십시오.

`NSLocationWhenInUseUsageDescription` 을 키로 입력하고, 값에는 접근을 요청할 사용자 친화적인 메세지를 사용하십시오.

다음으로, `NSExtension` 과 `NSExtensionAttributes` 를 펼쳐서 당신의 키보드 익스텐션에서 사용 가능한 선택지들을 확인하십시오.

각 설정들은 꽤 자명하지만, 그것들에 대해 더 배우고 싶다면 [애플 문서](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212) 를 확인하십시오.

사용자의 위치 데이터를 얻기 위해 전체 접근을 요청하기 위해, `RequestsOpenAccess` 값을 `YES` 로 변경하십시오. 이것은 키보드를 설치할 때, 사용자가 접근을 허용할지 말지 결정할 수 있음을 뜻합니다.

당신은 이미 키보드를 설치했기 때문에, 수동으로 접근 권한을 주어야 할 것입니다. 디바이스에 마지막 변경 사항을 적용하기 위해 익스텐션을 빌드하고 실행하십시오.

앱을 최소화하고 *설정 > 일반 > 키보드 > 키보드 > MoreKeyboard* 로 이동하여 스위치를 켜십시오. 당신의 키보드에 전체 접근 권한을 주기 위해 대화상자에서 *허용* 을 탭하십시오.

마지막으로 익스텐션을 빌드하고 실행하십시오. 커스텀 키보드로 전환한 후, 위치 데이터에 대한 접근을 허용할지 묻는 메세지가 표시될 것입니다. *허용* 을 탭하고 다시 "sos" 를 시도하십시오.

그리고 거기 있습니다! 키보드가 성공적으로 당신의 현재 위치에 접근하여 텍스트 인풋 뷰에 그것을 삽입하고 있습니다!

#### 여기서 어디로 가야 할까?

이 튜토리얼의 맨 위 또는 맨 아래에 있는 링크를 사용하여 최종 프로젝트를 다운로드할 수 있습니다.

이러한 새로 발견된 키보드 익스텐션 지식으로, 당신은 이 세계에 정말로 유용한 어떤 것을 당신 방식대로 만들어낼 수 있습니다.

이것은 오직 당신이 키보드 익스텐션에 추가할 수 있는 모든 기능의 겉면을 훑은 것일 뿐입니다. 좀더 깊은 것을 원한다면, [애플 커스텀 키보드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) 를 확인하십시오.

따라와 주셔서 감사합니다!

