[Human Interface Guidelines - App Architecture - Onboarding](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/requesting-permission/)

# 권한 요청*Requesting Permission*

사용자들은 현재 위치, 달력, 연락처 정보, 미리 알림*reminders*, 사진을 포함한 개인 정보에 접근하기 위해 애플리케이션에 권한을 주어야 합니다. 사람들은 이 정보에 접근할 수 있는 애플리케이션을 사용하는 편리함을 잘 알고 있지만, 또한 개인 데이터를 제어할 수 있기를 기대합니다. 예를 들어, 사람들은 그들의 물리적 위치로 사진 태그에 자동으로 기록되게 하거나 근처 친구들을 찾을 수 있게 되는 것을 좋아하지만, 또한 이러한 기능을 비활성화할 수 있는 옵션을 원합니다.

### 애플리케이션이 명백하게 필요로 할 때만 개인 데이터를 요청하십시오.

개인 정보를 요청하는 데 있어서 의심을 나타내는 것은 자연스러운데, 특히 그것을 필요로 하는 것이 명백하지 않을 때 그렇습니다. 명백하게 개인 데이터를 필요로 하는 기능을 사용할 때 권한 요청이 일어나는 것을 확실하게 하십시오. 예를 들어, 애플리케이션은 위치 추적 기능을 활성화할 때만 현재 위치에 접근을 요청할 수 있습니다.

### 애플리케이션이 정보를 필요로 하는 이유를 설명하십시오.

시스템의 권한 요청 얼러트에 표시하기 위한, 목적 문자열*purpose string* 또는 사용 서술 문자*usage description string*으로 알려져 있는 사용자 정의 텍스트를 제공하고, 예시를 포함하십시오. 텍스트는 짧고 구체적으로 유지하고, 문장의 형태를 사용하며, 사람들이 압박감을 느끼지 않도록 예의 있게 하십시오. 애플리케이션 이름이 포함될 필요는 없습니다. 시스템은 이미 애플리케이션을 식별합니다. 개발자 가이드에서, [사용자 프라이버리 보호하기](https://developer.apple.com/documentation/uikit/core_app/protecting_the_user_s_privacy)를 참고하십시오.

- 좋은 목적 문자열 예시
  - 애플리케이션은 코 고는 소리를 감지하기 위해 밤 동안 당신을 기록합니다.
- 나쁜 목적 문자열 예시
  - 더 좋은 경험을 위해 마이크 접근이 필요함.
  - 마이크 접근 켜기.

### 애플리케이션이 기능하는 데 필요한 경우에 실행시 권한을 요청하십시오.

애플리케이션이 작동하는 데 그들의 개인 정보에 의존하는 것이 명백하다면, 사용자는 이 요청으로 인해 고민하지 않을 것입니다.

### 쓸데 없이 위치 정보를 요청하지 마십시오.

위치 정보에 접근하기 전, 시스템을 확인하여 위치 서비스*Location Services*가 활성화되어 있는지 확인하십시오. 이 지식을 바탕으로, 기능이 정말로 그것을 필요로 할 때까지 얼러트를 지연시킬 수 있으며, 또는 얼러트를 완전히 피할 수도 있습니다. 위치 관련 기능을 구현하는 방법을 학습하기 위해, [MapKit](https://developer.apple.com/documentation/mapkit)과 [위치와 지도 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/Introduction/Introduction.html)를 참고하십시오.