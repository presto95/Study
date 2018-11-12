[Human Interface Guidelines - User Interaction - Audio](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/audio/)

# 오디오*Audio*

사람들은 볼륨 버튼, 무음 스위치*silence switch*, 헤드폰 컨트롤, 그리고 화면 상의 볼륨 슬라이더를 통해 소리를 조절합니다. 많은 써드 파티 악세사리 또한 사운드 컨트롤을 포함합니다. 오디오는 내장 또는 외장 스피커, 헤드폰, 심지어 무선으로는 AirPlay가 활성화된 기기나 블루투스 기기를 통해 출력될 수 있습니다. 소리가 애플리케이션의 경험이나 장식에 대해 주요한 양상이라면, 어떻게 사람들이 사운드가 행동할지 예상하고 그들의 기대를 충족시킬지 알 필요가 있습니다.

## 무음*Silence*

사람들은 벨소리나 메세지 수신 사운드와 같은, 기대하지 않는 소리에 방해받지 않기 위해 디바이스를 무음 모드로 바꿉니다. 또한 키보드 소리, 사운드 이펙트, 게임 사운드트랙, 그리고 다른 가청 피드백을 포함한 필수적이지 않은 소리를 비활성화 하기를 원합니다. 디바이스가 무음 모드로 설정될 때, 미디어 재생 중의 오디오, 알람, 그리고 오디오/비디오 메세징과 같은 명시적으로 개시된 소리만 발생해야 합니다.

## 볼륨*Volume*

디바이스의 물리 버튼을 사용하든 화면 상의 슬라이더를 사용하든, 사람들은 음악과 인앱 사운드 효과를 포함한 시스템 전체에서의 모든 사운드에 볼륨의 변화에 영향을 미칠 것을 기대합니다. 유일한 예외는 벨소리 볼륨으로, 오디오가 활발하게 재생되고 있지 않을 때 항상 분리되어 조절됩니다.

## 헤드폰*Headphones*

사람들 개인적으로 소리를 듣고 손에서 자유로워지게 하기 위해 헤드폰을 사용합니다. 헤드폰을 꼽을 때, 사용자들은 중단 없이 자동으로 소리가 다시 루트를 변경하기를 기대합니다. 헤드폰을 뽑을 때, 재생이 즉시 중지되기를 기대합니다.

## 훌륭한 오디오 경험 디자인하기*Designing a Great Audio Experience*

### 필요할 때 자동으로 수준을 조절하지만, 전체 볼륨에 대해서는 그렇게 하지 마십시오.

애플리케이션은 훌륭한 오디오 믹스를 달성하기 위해 상대적으로 독립적인 볼륨 수준을 조절할 수 있습니다. 하지만, 최종 출력은 항상 시스템 볼륨에 의해 결정되어야 합니다.

### 적절할 때에 오디오의 출력 방향을 다시 설정하는 것을 허가하십시오.

사람들은 종종 다른 오디오 출력 디바이스를 선택하는 것을 원합니다. 예를 들어, 거실의 스테레오 시스템, 자동차 오디오, 또는 Apple TV를 통해 음악을 듣고 싶어할 수 있습니다. 반드시 그렇게 해야 하는 이유가 없을지라도 이 능력을 지원하십시오.

### 오디오 조절을 허용할 때 시스템이 제공하는 볼륨 뷰를 사용하십시오.

오디오를 조절하기 위한 인터페이스 컨트롤을 제공하는 가장 좋은 방법은 볼륨 뷰를 사용하는 것입니다. 이 뷰는 커스터마이징 할 수 있으며, 볼륨 수준 슬라이더를 포함하고, 심지어 오디오 출력 방향을 재설정하는 컨트롤도 포함합니다. 개발자 가이드에서, [MPVolumeView](https://developer.apple.com/documentation/mediaplayer/mpvolumeview)를 참고하십시오.

### 짧은 소리와 진동에 대하여 시스템의 사운드 서비스를 사용하십시오.

개발자 가이드에서, [시스템 사운드 서비스](https://developer.apple.com/documentation/audiotoolbox/system_sound_services)를 참고하십시오.

### 소리가 애플리케이션에 필수적이라면, 오디오를 범주화 하십시오.

다른 오디오 범주는 소리가 무음 스위치에 의해 무음이 되거나, 다른 오디오와 섞거나, 애플리케이션이 백그라운드에 있는 동안 재생되게 합니다. 그 의미와 디바이스의 현재 오디오 상태에 기반한 범주를고르고, 그것을 오디오 세션에 할당하십시오. 예를 들어, 필요하지 않다면 또다른 앱에서 음악을 듣는 것을 멈추게 하지 마십시오. 일반적으로, 녹음이나 다른 시간에 오디오를 다시 재생하는 애플리케이션을 제외하면, 애플리케이션이 실행중인 동안 범주를 변경하지 않는 것이 가장 좋습니다. 개발자 가이드에서, [오디오 세션 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/Introduction/Introduction.html)를 참고하십시오.

| 카테고리                           | 의미                                                         | 행동                                                         |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Solo Ambient<br />(홀로 잔잔한)    | 소리는 필수적이지 않으나, 다른 오디오를 침묵시킨다. <br />사운드 트랙이 포함된 게임이 그 예시. | 무음 스위치에 응답한다. <br />다른 사운드와 섞이지 않는다. <br />백그라운드에서 재생되지 않는다. |
| Ambient<br />(잔잔한)              | 소리는 필수적이지 않고, 다른 오디오를 침묵시키지 않는다. <br />게임의 사운드트랙이 위치하는 게임 플레이 도중 또다른 애플리케이션에서 음악을 재생할 수 있게 하는 게임이 그 예시. | 무음 스위치에 응답한다. <br />다른 사운드와 섞인다. <br />백그라운드에서 재생되지 않는다. |
| Playback<br />(재생)               | 소리는 필수적이며 다른 오디오와 섞일 수 있다. <br />애플리케이션을 떠난 후에도 들을 수 있기를 원하는, 오디오북이나 외국어를 가르치는 교육용 애플리케이션이 그 예시. | 무음 스위치에 응답하지 않는다.<br />다른 소리와 섞이거나 섞이지 않을 수 있다.<br />백그라운드에서 재생 가능하다. |
| Record<br />(기록)                 | 소리는 녹음된다. 오디오 레코드 모드를 제공하는 노트 필기 애플리케이션이 그 예시. <br />이러한 성질의 애플리케이션은 사람들이 녹음된 노트를 재생하게 한다면 그 범주를 플레이백으로 변경할 수 있다. | 무음 스위치에 응답하지 않는다.<br />다른 소리과 섞이지 않는다.<br />백그라운드에서 녹음될 수 있다. |
| Play and record<br />(재생과 기록) | 소리는 녹음되고 재생되며, 잠재적으로 동시에 이루어질 수 있다. <br />오디오 메세징 또는 비디오 전화 애플리케이션이 그 예시. | 무음 스위치에 응답하지 않는다.<br />다른 소리와 섞이거나 섞이지 않을 수 있다.<br />백그라운드에서 녹음하고 재생할 수 있다. |

### 차단이 일어난 후 적절할 때 오디오 재생을 재개하십시오.

때때로, 현재 재생 중인 오디오는 다른 애플리케이션의 오디오에 의해 차단될 수 있습니다. 전화 수신과 같은 일시적인 차단은 재개 가능한 것으로 고려됩니다. Siri에 의해 개시된 음악 플레이리스트와 같은 영구적인 차단은 재개 가능하지 않은 것으로 고려됩니다. 재개 가능한 차단이 일어날 때, 애플리케이션은 오디오가 차단이 시작될 때 활발하게 재생되고 있었다면, 차단이 종료될 때 재생을 재개해야 합니다. 예를 들어, 사운드트랙을 재생하는 게임과 오디오를 재생하는 프로세스가 있는 미디어 애플리케이션 모두 재개되어야 합니다.

### 애플리케이션이 일시적인 오디오 재생을 마칠 때 다른 애플리케이션이 알게 하십시오.

애플리케이션이 일시적으로 다른 애플리케이션의 오디오를 차단할 수 있다면, 다른 애플리케이션이 재개하기 안전할 때 알림을 받을 수 있도록 적절하게 오디오 세션에 플래그를 달아야 합니다. 개발자 가이드에서, [AVFoundation](https://developer.apple.com/documentation/avfoundation)에 있는 [AVAudioSessionSetActiveOptionNotifyOthersOnDeactivation](https://developer.apple.com/documentation/avfoundation/avaudiosessionsetactiveoptions/avaudiosessionsetactiveoptionnotifyothersondeactivation) 상수를 참고하십시오.

### 의미 있는 경우에만 오디오 컨트롤에 응답하십시오.

사람들은 컨트롤 센터*Control Center*나 헤드폰에 있는 컨트롤과 같은 애플리케이션의 인터페이스 바깥에서, 애플리케이션이 포어그라운드에 있든 백그라운드에 있든 상관하지 않고, 오디오 재생을 컨트롤할 수 있습니다. 애플리케이션이 활발하게 오디오를 재생하고 있고, 명백하게 오디오과 관련된 컨텍스트에 있거나, AirPlay가 활성화된 디바이스와 연결되어 있다면, 오디오 컨트롤에 응답해도 좋습니다. 그렇지 않으면, 애플리케이션은 컨트롤이 활성화되어 있을 때 재생 가능한 또다른 애플리케이션의 오디오를 중지해서는 안됩니다.

### 오디오 컨트롤이 다른 목적을 갖게 하지 마십시오.

사람들은 오디오 컨트롤이 모든 애플리케이션에서 일관되게 동작할 것임을 기대합니다. 오디오 컨트롤의 의미를 절대 다시 정의하지 마십시오. 애플리케이션이 특정 컨트롤을 지원하지 않는다면, 그것들에 응답하지 않으면 됩니다.