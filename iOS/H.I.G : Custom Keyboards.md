[Human Interface Guidelines - Extensions - Custom Keyboards](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/custom-keyboards/)

# 커스텀 키보드*Custom Keyboards*

키보드 익스텐션은 기본 키보드를 커스텀 키보드로 교체합니다. 커스텀 키보드는 설정 앱의 *일반 > 키보드* 에서 활성화할 수 있습니다. 일단 활성화되면, 그 키보드는 어느 앱에서든 텍스트 입력 중에 사용 가능한데, 보안 텍스트 필드와 전화번호 필드를 수정할 때는 제외됩니다. 사람들은 여러 개의 커스텀 키보드를 활성화할 수 있고, 언제든지 그것들 사이에서 전환할 수 있습니다.

### 정말로 커스텀 키보드를 필요로 하는지 확인하십시오.

커스텀 키보드는 당신이 시스템 전체에서 유일한 키보드 기능을 노출시키기를 원할 때 말이 되는데, 텍스트를 입력하는 새로운 방법 또는 iOS에서 지원하지 않는 언어로 타이핑하는 기능 등을 제공하려고 할 때 그렇습니다. 만약 당신이 오직 당신의 애플리케이션 안에서만 커스텀 키보드를 사용하기를 원한다면, 대신 커스텀 인풋 뷰*custom input view* 를 만드는 것을 고려하십시오. [커스텀 인풋 뷰](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/custom-keyboards/#custom-input-views) 를 참고하십시오.

### 키보드 간 교체가 가능한 명백하고 쉬운 방법을 제공하십시오.

사람들은 기본 iOS 키보드에 있는 지구본 키를 알고 있는데, 이것은 당신이 여러 가지 키보드를 활성화했을 때 이모지 키에서 교체되고, 다른 키보드로 빠르게 전환할 수 있게 합니다. 그들은 당신의 키보드에서 비슷한 직관적인 경험을 기대합니다. 여러 키보드가 활성화되어 있으면 지구본 키가 이모지 키를 대체한다는 것을 알아두십시오.

### 시스템이 제공하는 키보드 특징을 복제하지 마십시오.

iPhone X에서, 이모지 / 지구본, 그리고 받아쓰기 키는 자동으로 키보드 아래에 나타나는데, 당신이 커스텀 키보드를 사용중일 때도 그렇습니다. 당신의 애플리케이션은 이러한 키들에 영향을 미칠 수 없으므로, 그것들을 당신의 키보드에 반복함으로서 발생할 수 있는 혼란을 피하십시오.

### 당신의 애플리케이션에 키보드 튜토리얼을 제공하는 것을 고려하십시오.

사람들은 기본 키보드에 익숙해져 있고, 새로운 키보드 사용 방법을 익히는 것은 시간이 드는 일입니다. 키보드 자체가 아닌 애플리케이션에서 튜토리얼을 제공하여 쉽게 실사용 과정를 익힐 수 있습니다. 사람들에게 당신의 키보드를 활성화하는 방법, 텍스트 입력 중 활성화하는 방법, 사용하는 방법, 기본 키보드로 돌아가는 방법을 알려주십시오.

개발자 가이드에서, [애플리케이션 확장 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/index.html) 내의 [커스텀 키보드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) 를 확인하십시오.

## 커스텀 인풋 뷰*Custom Inpuy Views*

커스텀 인풋 뷰는 기본 키보드를 커스텀 키보드로 교체하지만, 시스템 전체가 아닌 오직 당신의 애플리케이션 안에서만 그렇습니다. 유일하고 능률적인 데이터 입력 방법을 제공하기 위해 커스텀 인풋 뷰를 사용하십시오. 예를 들어, `Numbers` 앱은 스프레드시트를 편집하는 동안 숫자 값을 입력하기 위한 커스텀 인풋 뷰를 구현합니다.

### 기능을 명백하게 하십시오.

커스텀 인풋 뷰에 있는 컨트롤들은 당신의 애플리케이션의 문맥에 맞아야 합니다. 데이터 입력은 명백하고 직관적이어야 하므로, 부가적인 지침은 불필요합니다.

### 타이핑 중 기본 키보드 클릭 사운드를 내십시오.

키보드 클릭 소리는 사용자가 키보드에 있는 키를 누르는 동안 청각적 피드백을 제공합니다. 당신의 인풋 뷰에 있는 커스텀 컨트롤을 누르는 것도 또한 이 소리를 제공해야 합니다. 이 소리는 오직 가시적인 커스텀 인풋 뷰에만 사용가능하며, 사람들은 설정 > 소리 에서 시스템 전체 소리를 비활성화할 수 있음을 알아두십시오. 개발자 가이드에서 [UIDevice](https://developer.apple.com/documentation/uikit/uidevice) 의 [playInputClick](https://developer.apple.com/documentation/uikit/uidevice/1620050-playinputclick) 메소드를 확인하십시오.

### 가능하다면 인풋 악세사리 뷰를 제공하십시오.

몇몇 애플리케이션은 부가적인 커스텀 인풋 악세사리 뷰를 구현하여 키보드 위에 나타납니다. Numbers 앱에서, 인풋 악세사리 뷰는 사람들이 기본 또는 사용자 정의 계산을 입력하는 것을 도와줍니다.

개발자 가이드에서, [iOS 텍스트 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) 의 [커스텀 데이터 인풋 뷰](https://developer.apple.com/library/content/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html) 를 확인하십시오.