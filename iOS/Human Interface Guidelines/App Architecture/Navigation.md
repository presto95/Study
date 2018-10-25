[Human Interface Guidelines - App Architecture - Navigation](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/navigation/)

# 내비게이션*Navigation*

사람들은 그들의 기대를 충족시키지 않을 때까지 애플리케이션의 내비게이션에 대해 인지하지 못하는 경향이 있습니다. 당신의 일은 그 자체에 대한 집중을 불러일으키지 않으면서 애플리케이션의 구조와 목적을 지원하는 방식의 내비게이션을 구현하는 것입니다. 내비게이션은 자연적이고 친숙하게 느껴져야 하며, 인터페이스를 지배하거나 컨텐로부터 포커스를 멀리 두게 해서는 안됩니다. iOS에서는 세 가지 주요한 내비게이션 스타일이 있습니다.

## 계층적 내비게이션*Hierarchical Navigation*

목적지에 도달할 때까지 화면 별로 선택하게 합니다. 또다른 목적지로 가기 위해, 스텝을 되돌아 가거나 처음부터 다시 시작하여 다른 선택을 해야 합니다. 설정 앱과 메일 앱이 이 내비게이션 스타일을 사용합니다.

## 평평한 내비게이션*Flat Navigation*

다중 컨텐츠 범주 사이에서 이리저리 움직입니다. 음악 앱과 앱 스토어 앱이 이 내비게이션 스타일을 사용합니다.

## 컨텐츠 주도 또는 경험 주도 내비게이션*Content-Driven or Experience-Driven Navigation*

컨텐츠를 통해 자유롭게 움직이거나, 컨텐츠 그 자체가 내비게이션을 정의합니다. 게임, 책, 그리고 다른 몰입적인 애플리케이션이 일반적으로 이 내비게이션 스타일을 사용합니다.

몇몇 애플리케이션은 하나 이상의 내비게이션 스타일을 조합합니다. 예를 들어, 평평한 내비게이션을 사용하는 애플리케이션은 각각의 범주 내에서 계층적 내비게이션을 구현할 수 있습니다.

### 항상 명백한 경로를 제공하십시오.

사람들은 그들이 애플리케이션 안에서 어디에 있고 어떻게 다음 목적지에 도달할 수 있는지 항상 알고 있어야 합니다. 내비게이션 스타일에 관계 없이, 컨텐츠를 통한 경로가 논리적이고, 예측 가능하며, 따라가기 쉬워야 합니다. 일반적으로, 각 화면에서 하나의 경로를 제공하십시오. 하나 이상의 컨텍스트가 있는 화면을 볼 필요가 있다면, 액션 시트, 얼러트, 팝오버, 또는 모달 뷰를 사용하는 것을 고려하십시오. 더 알아보기 위해, [액션 시트](https://developer.apple.com/design/human-interface-guidelines/ios/views/action-sheets/), [얼러트](https://developer.apple.com/design/human-interface-guidelines/ios/views/alerts/), [팝오버](https://developer.apple.com/design/human-interface-guidelines/ios/views/popovers/), 그리고 [모달리티](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/modality/)를 참고하십시오.

### 컨텐츠를 얻는 것을 빠르고 쉽게 하는 정보 구조를 디자인 하십시오.

탭, 스와이프, 화면의 숫자를 최소화하는 방식으로 정보 구조를 조직하십시오.

### 유려함을 만들기 위해 터치 제스처를 사용하십시오.

최소의 충돌로 이너페이스를 통해 이동하는 것을 쉽게 합니다. 예를 들어, 이전 화면으로 돌아가기 위해 화면의 사이드로부터 스와이프하게 할 수 있습니다.

### 표준 내비게이션 컴포넌트를 사용하십시오.

가능할 때마다, 페이지 컨트롤*page controls*, 탭 바*tab bars*, 세그먼티드 컨트롤*segmented controls*, 테이블 뷰*table views*, 컬렉션 뷰*collection views*, 그리고 스플릿 뷰*split views*와 같은 표준 내비게이션 컨트롤을 사용하십시오. 사용자들은 이미 이러한 컨트롤에 익숙해져 있고, 어떻게 애플리케이션 주위를 돌아다닐 수 있는지 직관적으로 알게 될 것입니다.

### 데이터의 계층을 가로지르기 위해 내비게이션 바를 사용하십시오.

내비게이션 바의 타이틀은 계층에서의 현재 위치를 보여줄 수 있고, 백 버튼은 이전 위치로 되돌아가는 것을 쉽게 해줍니다. 특정 가이드를 위해, [내비게이션 바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/navigation-bars/)를 참고하십시오.

### 컨텐츠나 기능에 있어서 동등한 범주를 나타내기 위해 탭 바를 사용하십시오.

탭 바는 현재 위치에 관계 없이, 범주 사이를 가로지르는 것을 빠르고 쉽게 합니다. 특정 가이드를 위해, [탭 바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/tab-bars/)를 참고하십시오.

### 같은 형식의 컨텐츠를 여러 개 갖는 페이지가 있다면 페이지 컨트롤을 사용하십시오.

페이지 컨트롤은 이용 가능한 페이지 수와 현재 활성화된 것을 명백하게 알려줍니다. 날씨 앱은 위치에 특정한 날씨 페이지를 표시하기 위해 페이지 컨트롤을 사용합니다. 특정 가이드를 위해, [페이지 컨트롤](https://developer.apple.com/design/human-interface-guidelines/ios/controls/page-controls/)을 참고하십시오.

### 팁

세그먼티드 컨트롤과 툴바는 내비게이션을 활성화하지 않습니다. 정보를 다른 범주에 구성하기 위해 세그먼티드 컨트롤을 사용하십시오. 현재 컨텍스트와 상호 작용하기 위한 컨트롤을 제공하기 위해 툴바를 사용하십시오. 이러한 타입의 요소에 대한 추가적인 정보를 위해, [세그먼티드 컨트롤](https://developer.apple.com/design/human-interface-guidelines/ios/controls/segmented-controls/)과 [툴바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/toolbars/)를 참고하십시오.