[Human Interface Guidelines - System Capabilities - Multitasking](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/multitasking/)

# 멀티태스킹*Multitasking*

멀티태스킹은 iOS 디바이스에서 멀티태스킹 인터페이스를 통해, 또는 iPad에서 다중 손가락 제스처를 사용하여 언제든지 하나의 애플리케이션에서 다른 애플리케이션으로 빠르게 전환하게 해줍니다. iPad에서, 멀티태스킹은 또한 슬라이드 오버*Slide Over*, 스플릿 뷰*Split View*, 또는 사진 내 사진*Picture in Picture* 모드에서 한 번에 두 개의 애플리케이션을 사용할 수 있게 합니다. 슬라이드 오버는 현재 애플리케이션의 컨텍스트를 벗어나지 않고 일시적으로 두 번째 애플리케이션을 사용하기 위해 화면의 오른쪽 가장자리에서 스와이프하여 접근되는데, 사파리 앱을 사용하면서 메일 앱의 메일함을 빠르게 보기 위한 경우 그렇습니다. 스플릿 뷰는 동시에 두 개의 양 옆에 위치한 애플리케이션을 사용하게 하고, 사진 내 사진은 또다른 애플리케이션에서 작업하는 동안 비디오를 시청할 수 있게 합니다.

멀티태스킹 환경에서 번창하는 애플리케이션을 디자인하는 것은 디바이스의 다른 애플리케이션들과 조화롭게 공존하는 애플리케이션에 달려있습니다. 이것은 애플리케이션이 너무 많은 CPU, 메모리, 화면 공간, 또는 다른 시스템 자원을 사용해서는 안된다는 것을 의미합니다. 갑작스런 방해와 다른 애플리케이션에서의 오디오, 백그라운드로 또는 백그라운드로부터 빠르고 부드럽게 전환되는 것, 백그라운드에서 동작할 때 확실하게 작동하는것에 잘 응답해야 합니다.

### 중단에 대비하고 재개할 준비를 하십시오.

애플리케이션은 언제든지 방해받을 수 있습니다. 방해가 발생할 때, 애플리케이션은 현재 상태를 빠르고 정확하게 저장하여 사람들이 되돌아올 때 떠난 위치에서 끊어짐 없이 지속될 수 있게 해야 합니다. 개발자 가이드에서, [iOS 애플리케이션 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html)의 [실행 사이에서 애플리케이션의 시각적 모습 보존하기](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforImplementingYourApp/StrategiesforImplementingYourApp.html#//apple_ref/doc/uid/TP40007072-CH5-SW2)를 참고하십시오.

### 인터페이스가 이중 높이*double-high* 상태 표시줄과 함께 잘 작동하는지 확실하게 하십시오.

진행 중인 전화, 오디오 녹음, 테더링 같은 특정 기능은 화면 상단에 추가 상태 표시줄을 표시합니다. 준비되지 않은 애플리케이션에서, 이것은 높이를 추가시켜 다른 인터페이스 요소를 덮거나 아래로 밀어버리는 레이아웃 문제를 유발할 수 있습니다. 이러한 기능을 사용하여 애플리케이션을 테스트하는 것은 인터페이스가 적절하게 반응하고 여전히 좋게 보이는 것을 보장하는 것을 가능하게 합니다.

### 주의나 활동적인 참가를 필요로 하는 활동을 중지하십시오.

예를 들어, 애플리케이션이 게임이나 미디어 시청 애플리케이션이면, 또다른 애플리케이션으로 전환할 때 어느 것도 놓치고 싶지 않다는 것을 확실하게 하십시오. 되돌아왔을 때, 마치 떠나지 않았던 것처럼 지속되게 하십시오.

### 외부 오디오에 적절하게 반응하십시오.

때때로, 애플리케이션의 오디오는 또다른 애플리케이션이나 시스템 그 자체의 오디오로부터 방해받을 수 있습니다. 예를 들어, 다가오는 전화 수신이나 Siri에 의해 개시된 음악 플레이리스트는 애플리케이션의 오디오에 개입할 수 있습니다. 이러한 상황이 발생할 때, 애플리케이션의 응답은 사람들의 기대를 충족시켜야 합니다. 음악, 팟캐스트, 또는 오디오북을 재생하는 것과 같은 주요한 오디오 개입에 대해서, 애플리케이션은 자신의 오디오를 무기한 중지시켜야 합니다. GPS 방향 지시 알림과 같은 짧은 개입에 대해서, 애플리케이션은 일시적으로 자신의 오디오 볼륨을 낮추거나 오디오를 중지시키고 개입이 끝났을 때 재개해야 합니다. 추가적인 가이드를 위해, [오디오](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/audio/)를 확인하십시오.

### 백그라운드에서 사용자가 개시한 작업을 끝내십시오.

누군가가 작업을 시작했을 때, 애플리케이션 바깥으로 전환하더라도 작업이 끝나기를 기대합니다. 애플리케이션이 추가적인 입력을 필요로 하지 않는 작업을 중간 정도 수행하고 있다면, 중단되기 전 백그라운드에서 완료되게 하십시오.

### 알림은 드물게 사용하십시오.

애플리케이션은 그것이 중지되었거나, 백그라운드에서 실행중이거나, 아예 실행중이지 않을 때와 같은 특정 시간에 알림을 보내도록 조정할 수 있습니다. 알림은 중요한 정보를 소통하는 데 좋지만, 너무 많은 알림으로 사람들을 힘들게 하지 마십시오. 예를 들어, 애플리케이션이 백그라운드에서 작업을 마칠 때마다 알림을 표시하지 마십시오. 대신, 애플리케이션으로 돌아와서 작업을 확인하도록 하십시오. 추가적인 가이드를 위해, [알림](https://developer.apple.com/design/human-interface-guidelines/ios/features/notifications/)을 참고하십시오.

iPad에 특정한 개발자 가이드를 위해, [iPad에서 멀티태스킹 향상 채택하기](https://developer.apple.com/library/content/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html)를 참고하십시오.