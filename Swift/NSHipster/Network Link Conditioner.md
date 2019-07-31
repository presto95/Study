# Network Link Conditioner

제품 설계는 공감에 대한 것입니다. 사용자가 원하는 것, 좋아하는 것, 싫어하는 것, 혼란스럽게 하는 것을 알고 그러한 동기를 이해하고 구체화하는 것을 배우는 것. 이것은 무언가를 훌륭하게 만들기 위해 필요한 것입니다.

그러므로 우리는 세계의 자체 운영 모델을 넘어서 닿는 것에 투자합니다. 우리는 우리의 경험을 다른 로케일에 맞게 조정합니다. 우리는 스크린 리더 또는 다른 보조 기술의 유용성 영향을 고려합니다. 우리는 이러한 기대에 반하는 구현을 지속적으로 평가합니다.

그러나 **네트워크 상태**는 앱 개발자가 종종 놓치는 중요한 요소 중 하나입니다. 더욱 상세하게 말하자면, 인터넷 연결의 대기 시간과 대역폭에 관한 것입니다.

사용자 경험에 필수 불가결한 어떤 것을 위해 대부분의 개발자가 다른 조건 하에서 앱을 필드 테스트 하기 위해 애드혹 접근법(특정 목적만을 위해 정형화된 것)을 취한다는 것은 좋지 않습니다.

이번 주에는 Network Link Conditioner에 대해 말할 것입니다. 이는 macOS와 iOS 기기에서 불리한 네트워크 환경을 정확하고 일관성 있게 제공하는 유틸리티입니다.

## Installation

Network Link Conditioner는 "Additional Tools for Xcode" 패키지에서 찾을 수 있습니다. Downloads for Apple Developers 페이지에서 다운로드할 수 있습니다.

"Additional Tools"를 찾고 패키지의 적절한 릴리즈를 선택하세요.

다운로드가 끝나면 DMG 파일을 열고 "Hardware" 디렉토리로 이동하여 "Network Link Condition.prefPane"을 더블 클릭하세요.

System Preferences 하단에 있는 Network Link Conditioner를 클릭하세요.

> macOS 10.14에서 Network Link Conditioner를 최초로 설치하면 모든 것이 기대한 대로 잘 동작할 것입니다. 하지만 System Preferences를 닫고 다시 열면 해당 환경 설정 패널은 더이상 보이지 않을 것이고, 다음의 에러 메세지와 함께 재설치를 유도할 것입니다.
>
> 임시 해결책으로, 사용자 `PreferencePanes` 디렉토리에서 시스템 수준 디렉토리로 해당 환경 설정 패널을 옮길 수 있습니다. `Terminal.app` 에서 다음의 명령을 입력하세요. (패스워드를 입력해야 할 것입니다.)
>
> ```sh
> $ sudo mv ~/Library/PreferencePanes/Network\ Link\ Conditioner.prefPane /Library/PreferencePanes/
> ```
>
> 이를 완료하면 Network Link Conditioner는 다음에 System Preferences를 열 때 보일 것입니다.

## Controlling Bandwidth, Latency, and Packet Loss

Network Link Conditioner를 활성화하여 선택된 구성에 따라 시스템 전체의 네트워크 환경을 변경할 수 있으며, 업링크 또는 다운로드 대역폭, 대기 시간 및 패킷 손실율을 제한할 수 있습니다.

다음의 프리셋 중 하나를 선택할 수 있습니다.

- 100% 손실
- 3G
- DSL
- EDGE
- High Latency DNS
- LTE
- 네트워크 상태 매우 나쁨
- WiFi
- WiFi 802.11ac

...또는 특정 요구 사항에 따라 당신만의 것을 만들 수 있습니다.

이제 Network Link Conditioner를 활성화한 후 앱을 실행시켜 보세요.

네트워크 지연 시간이 앱의 시작에 얼마나 많은 영향을 미치나요? 대역폭이 테이블 뷰 스크롤 성능에 어떤 영향을 미치나요? 앱이 100% 패킷 손실에도 잘 동작하나요?

## Enabling Network Link Conditioner on iOS Devices

환경 설정 패널이 시뮬레이터에서 개발하는 것을 위해 잘 동작할지라도, 실기기에서 테스트하는 것 또한 중요합니다. 다행스럽게도 Network Link Conditioner는 iOS에서도 사용 가능합니다.

iOS에서 Network Link Conditioner를 사용하려면 개발을 위해 당신의 기기를 설정해야 합니다.

- iOS 기기를 Mac에 연결
- Xcode에서 Window > Organizer로 이동
- 사이드바에서 기기 선택
- "Use for Development" 클릭

이제 당신은 설정 앱의 개발자 섹션에 접근할 수 있을 것입니다. iOS 기기의 설정 > 개발자 > Networking에서 Network Link Conditioner를 활성화하고 구성할 수 있습니다. (테스트 후 끄는 것도 기억하세요!)

