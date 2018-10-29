[Human Interface Guidelines - Bars - Status Bars](https://developer.apple.com/design/human-interface-guidelines/ios/bars/status-bars/)

# 상태 바*Status Bars*

상태 바는 화면의 상단 가장자리를 따라 나타나며 시간, 통신 사업자*cellular carrier*, 네트워크 상태 및 배터리 수준과 같은 디바이스의 현재 상태에 대한 유용한 정보를 표시합니다. 상태 바에 나타나는 실제 정보는 디바이스와 시스템 구성에 따라 다양합니다.

### 시스템이 제공하는 상태 바를 사용하십시오.

사람들은 상태 바가 시스템 도처에서 일관될 것이라고 기대합니다. 커스텀 상태 바로 그것을 대체하지 마십시오.

### 상태 바 스타일을 애플리케이션의 디자인과 어울리게 하십시오.

상태 바의 텍스트와 표시기의 시각적 스타일은 밝거나 어두울 수 있으며, 애플리케이션에서 전체적으로 또는 다른 화면에서 개별적으로 설정될 수 있습니다. 어두운 상태 바는 밝은 색상을 사용하는 컨텐츠에서 잘 작동하며 밝은 상태 바는 어두운 색상을 사용하는 컨텐츠에서 잘 작동합니다.

### 상태 바 아래에 있는 컨텐츠를 명료하지 않게 하십시오.

기본적으로, 상태 바의 배경은 투명하여, 아래에 있는 컨텐츠가 그것을 통해 보일 수 있게 합니다. 상태 바를 읽을 수 있게 유지하고 그것 뒤에 있는 컨텐츠가 대화형인 것을 함축하지 마십시오. 이것을 하기 위한 몇 가지 일반적인 기술이 있습니다.

- 애플리케이션에 내비게이션 바를 사용하여, 자동으로 상태 바의 배경을 표시하고, 컨텐츠가 상태 바 아래에 나타나지 않는다는 것을 보장하십시오.
- 상태 바 뒤에 그래디언트 또는 속이 가득 찬 색상과 같은 커스텀 이미지를 표시하십시오.
- 상태 바 뒤에 흐린 효과가 적용된 뷰를 배치하십시오. 개발자 가이드에서, [UIBlurEffect](https://developer.apple.com/documentation/uikit/uiblureffect)를 참고하십시오.

### 전체 화면 미디어를 표시할 때 상태 바를 일시적으로 숨기는 것을 고려하십시오.

상태 바는 사용자가 미디어에 집중하려고 할 때 주의를 산만하게 할 수 있습니다. 일시적으로 이러한 요소를 숨겨 더 몰입감 있는 경험을 제공하십시오. 예를 들어, 사진 앱은 사용자가 전체 화면 사진을 둘러볼 때 상태 바와 다른 인터페이스 요소를 숨깁니다.

### 영구적으로 상태 바를 숨기지 마십시오.

상태 바 없이, 사람들은 시간을 확인하거나 Wi-Fi가 연결되어 있는지 확인하기 위해 애플리케이션을 떠나야 합니다. 사람들이 단순하고 발견할 수 있는 제스처를 사용하여 숨겨진 상태 바를 다시 표시하게 하십시오. 사진 앱에서 전체 화면 사진을 둘러볼 때 한 번 탭하여 상태 바를 다시 보이게 합니다.

### 네트워크 액티비티를 나타내기 위해 상태 바를 사용하십시오.

애플리케이션이 네트워크를 사용할 때, 특히 긴 작업일 때, 네트워크 액티비티 상태 바 표시기를 보여 사람들이 액티비티가 일어나고 있음을 알게 하십시오. [네트워크 액티비티 표시기](https://developer.apple.com/design/human-interface-guidelines/ios/controls/progress-indicators/#network-activity-indicators)를 참고하십시오.

개발자 가이드에서, [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)의 [UIStatusBarStyle](https://developer.apple.com/documentation/uikit/uistatusbarstyle) 상수 및 [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)의 [preferredStatusBarStyle](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621416-preferredstatusbarstyle) 프로퍼티를 참고하십시오.