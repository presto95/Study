[Human Interface Guidelines - App Architecture - Settings](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/settings/)

# 설정*Settings*

몇몇 애플리케이션은 설정 또는 구성에 관한 선택을 할 수 있게 하는 방법을 제공할 필요가 있지만, 대부분의 애플리케이션은 그렇게 하는 것을 못하게 하거나 지연시킬 수 있습니다. 성공적인 애플리케이션은 경험에 적응하기 위한 몇 가지 편리한 방법을 제공하면서, 대부분의 사람들에게 올바르게 잘 동작합니다. 대부분의 사람들이 기대하는 기능을 하도록 애플리케이션을 디자인할 때, 설정을 위한 필요를 줄여야 합니다.

### 시스템에서 할 수 있는 것을 추론하십시오.

사용자, 디바이스, 또는 환경에 대한 정보를 필요로 한다면, 사용자에게 묻는 대신 가능할 때마다 그것에 대해 시스템에게 조회하십시오. 예를 들어, 지역 옵션을 나타내기 위해 누군가에게 우편 번호를 입력하는 것을 요청하는 대신, 현재 위치에 대한 권한을 요청하십시오.

### 애플리케이션 내의 구성 옵션에 사려 깊게 우선 순위를 부여하십시오.

애플리케이션의 메인 화면은 필수적이거나 주기적으로 변화하는 옵션을 위치하기 좋은 자리입니다. 부차적인 화면은 단지 때때로 변화하는 옵션에 더 좋습니다.

### 설정 앱에서 변경된 구성 옵션을 드물게 드러내십시오.

설정 앱은 시스템 도처의 구성 변화를 만드는 중앙 위치입니다. 하지만 사람들은 그 곳에 접근하기 위해 애플리케이션을 떠나야 합니다. 애플리케이션 내에서 직접적으로 설정을 조정하는 것이 훨씬 더 편리합니다. 드물게 변화를 요구하는 설정을 제공해야 한다면, 개발자 가이드에서 [선호도 및 설정 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html)의 [iOS 설정 앱 번들 구현하기](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html)를 참고하십시오.

### 적절한 경우 설정 앱에 대한 바로 가기를 제공하십시오.

"설정으로 이동 > MyApp > 개인 정보 > 위치 서비스"와 같은, 애플리케이션이 사용자들을 설정 앱으로 이끄는 텍스트를 포함한다면, 자동으로 그 위치를 여는 버튼을 제공하십시오. 개발자 가이드에서, [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)의 [openSettingsURLString](https://developer.apple.com/documentation/uikit/uiapplication/1623042-opensettingsurlstring)을 참고하십시오.