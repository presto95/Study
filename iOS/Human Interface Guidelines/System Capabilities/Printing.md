[Human Interface Guidelines - System Capabilities - Printing](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/printing/)

# 인쇄*Printing*

애플리케이션은 이미지, PDF, 그리고 호환되 프린터에 대한 다른 컨텐츠를 무선으로 인쇄할 수 있는 시스템에 내장된 AirPrint 기술의 이점을 취할 수 있습니다. AirPrint가 가능한 애플리케이션에서 인쇄 가능한 컨텐츠를 보는 동안, 사람들은 일반적으로 내비게이션 바나 툴바에 있는 액션 버튼을 탭하고 프린터 뷰에 표시된 인쇄*Print* 액션을 탭할 것입니다. 이 뷰는 사용 가능한 프린터의 목록, 인쇄 장수와 페이지 범위와 같은, 어떠한 사용자 정의 옵션, 인쇄를 개시하기 위한 버튼을 제공합니다.

### 인쇄를 발견 가능하게 하십시오.

애플리케이션이 툴바 또는 내비게이션 바를 가지고 있다면, 시스템이 제공하는 액션*Action* 버튼을 통해 인쇄할 수 있게 하십시오. 사용자들은 이 버튼에 익숙하고 다른 애플리케이션에서 인쇄하기 위해 그것을 사용합니다. 애플리케이션이 툴바나 내비게이션 바를 가지고 있지 않다면, 대신 커스텀 인쇄 버튼을 디자인 하십시오.

### 인쇄 가능할 때만 인쇄를 가능하게 하십시오.

화면 상에 프린트할 수 있는 것이 없고 사용 가능한 프린터가 없다면, 누군가가 액션 버튼을 누를 때 인쇄 액션을 보여주지 마십시오. 애플리케이션이 커스텀 인쇄 버튼을 구현한다면, 인쇄가 가능하지 않을 때 그것을 비활성화하거나 숨기십시오.

### 값을 더하는 인쇄 옵션을 제공하십시오.

사람들이 당신의 컨텐츠를 인쇄할 때 특정하기 원할 만한 옵션에 대해 생각하십시오. 페이지 범위를 선택하고 다중 인쇄를 요청하는 옵션을 고려하십시오. 말이 되고 프린터가 지원한다면, 양면 인쇄와 같은 추가 옵션을 활성화 하십시오.

개발자 가이드에서, [iOS에서의 그리기와 인쇄 가이드](https://developer.apple.com/library/content/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/Introduction/Introduction.html)와 [UIPrintInteractionController](https://developer.apple.com/documentation/uikit/uiprintinteractioncontroller)를 참고하십시오.