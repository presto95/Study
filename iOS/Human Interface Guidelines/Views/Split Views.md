[Human Interface Guidelines - Views - Split Views](https://developer.apple.com/design/human-interface-guidelines/ios/views/split-views/)

# 스플릿 뷰*Split Views*

스플릿 뷰는 두 개의 나란한 컨텐츠 평면*pane*의 표현을 관리하는데, 주요 평면에는 지속되는 컨텐츠, 보조 평면에는 관련된 정보를 나타냅니다. 각각의 평면은 내비게이션 바, 툴바, 탭 바, 테이블, 컬렉션, 이미지, 지도 및 커스텀 뷰를 포함하는 어떠한 다양한 요소라도 포함할 수 있습니다. 스플릿 뷰는 종종 필터 가능한 컨텐츠와 함께 사용될 수 있습니다. 필터 범주의 목록은 주요 평면에 나타나며, 선택된 범주에 대한 필터링된 결과는 보조 평면에 보여집니다. 애플리케이션이 그것을 필요로 한다면, 주요 평면은 보조 평면을 오버레이 할 수 있으며 사용되지 않을 때는 화면 밖에서 숨길 수 있습니다. 이것은 특히 디바이스가 세로 방향에 있을 때 유용한데, 보조 평면에서 컨텐츠를 보여주기 위한 더 많은 공간을 허용하기 때문입니다. 관련된 가이드에서, [오토레이아웃](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#auto-layout)을 참고하십시오.

### 당신의 컨텐츠와 잘 작동하는 스플릿 뷰 레이아웃을 선택하십시오.

기본적으로, 스플릿 뷰는 화면의 1/3을 주요 평면에, 2/3을 보조 평면에 바칩니다. 그 화면은 또한 반으로 분리될 수 있습니다. 당신의 컨텐츠에 기반한 적절한 분리를 선택하여, 평면이 균형을 잃은 것처럼 보이지 않는 것을 확실하게 하십시오. 주요 평면보다 좁은 보조 평면을 만들지 마십시오.

### 주요 평면에서 항상 활성화된 선택을 강조하십시오.

보조 평면의 컨텐츠가 변경 가능할지라도, 주요 평면에서 명백하게 식별 가능한 선택과 항상 상응해야 합니다. 이것은 평면 간 관계를 이해하는 것을 돕습니다.

### 일반적으로, 스플릿 뷰의 한 쪽으로의 탐색을 제한하십시오.

스플릿 뷰의 양 평면에 탐색을 배치하는 것은 방향을 잡는 것을 어렵게 하고 두 평면 간 관계를 인식하기 힘들게 합니다.

### 숨겨진 주요 평면에 접근하는 많은 방법을 제공하십시오.

주요 평면이 화면 밖에 있을 수 있는 레이아웃에서, 평면을 드러내기 위해 일반적으로 내비게이션 바 안에 있는 버튼을 제공하는 것을 보장하십시오. 애플리케이션이 다른 기능을 수행하기 위해 스와이프 제스처를 사용하지 않는 한, 주요 평면에도 접근하기 위해 화면의 가장자리에서 스와이프하게 하십시오.

개발자 가이드에서, [UISplitViewController](https://developer.apple.com/documentation/uikit/uisplitviewcontroller)를 참고하십시오.