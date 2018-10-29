[Human Interface Guidelines - Bars - Toolbars](https://developer.apple.com/design/human-interface-guidelines/ios/bars/toolbars/)

# 툴바*Toolbars*

툴바는 애플리케이션 화면의 하단에 나타나 현재 뷰 또는 그것 내의 컨텐츠와 관련이 있는 액션을 수행하기 위한 버튼을 포함합니다. 툴바는 배경 틴트 색상을 가질 수 있고, 투명하며, 종종 사람들이 그것들을 필요로 하지 않다면 숨겨집니다. 예를 들어, 사파리 앱에서 툴바는 당신이 무언가를 읽기 위해 페이지 스크롤을 시작할 때 숨겨집니다. 스크린의 하단을 탭하여 그것을 다시 보이게 할 수 있습니다. 툴바는 또한 키보드가 화면에 나타날 때 숨겨집니다.

### 관련된 툴바 버튼을 제공하십시오.

툴바는 현재 컨텍스트에서 말이 되는 빈번하게 사용되는 명령을 포함합니다.

### 애플리케이션에서 아이콘이 올바른지, 타이틀이 있는 버튼이 올바른지 고려하십시오.

아이콘은 당신이 세 개 이상의 툴바 버튼을 필요로 할 때 잘 동작합니다. 세 개 이하의 버튼을 갖는다면, 때때로 텍스트가 더 명백할 수 있습니다. 예를 들어, 캘린더 앱에서 아이콘은 혼란스럽게 할 수 있기 때문에 텍스트가 사용됩니다. 텍스트의 사용은 또한 초대*Inbox* 버튼이 캘린더 및 이벤트 초대의 수를 보이는 것을 허용합니다.

### 툴바에 세그먼티드 컨트롤을 사용하지 마십시오.

세그먼티드 컨트롤은 컨텍스트 전환을 가능하게 하지만, 툴바는 현재 화면에 특화되어 있습니다. 컨텍스트를 전환하는 방법을 제공할 필요가 있다면, 탭 바를 대신 사용하는 것을 고려하십시오. [탭 바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/tab-bars/)를 참고하십시오.

### 텍스트 타이틀이 붙은 버튼에 충분한 공간을 제공하십시오.

툴바가 여러 개의 버튼을 포함할 때, 그러한 버튼의 텍스트는 함께 동작하는 것처럼 나타나, 버튼 구별을 힘들게 합니다. 버튼 사이에 고정된 공간을 삽입하여 분리하십시오. 개발자 가이드에서, [UIBarButtonItem](https://developer.apple.com/documentation/uikit/uibarbuttonitem)의 [UIBarButtonSystemItemFixedSpace](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/uibarbuttonsystemitemfixedspace) 상수값을 참고하십시오.

개발자 가이드에서, [UIToolBar](https://developer.apple.com/documentation/uikit/uitoolbar)를 참고하십시오.

**참고.** 탭 바와 툴바의 차이를 이해하는 것은 중요한데, 두 가지 타입의 바 모두 애플리케이션 화면의 하단에 나타나기 때문입니다. 탭 바는 시계 앱의 알람, 스톱워치, 타이머 탭과 같은, 애플리케이션의 다른 섹션 간 빠른 전환을 가능하게 합니다. 툴바는 아이템 생성, 아이템 삭제, 주석 달기 또는 사진 촬영하기와 같은, 현재 컨텍스트에 관련된 액션을 수행하기 위한 버튼을 포함합니다. [툴바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/toolbars/)를 참고하십시오. 탭 바와 툴바는 절대 같은 뷰에서 함께 나타나서는 안됩니다.