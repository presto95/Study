[Human Interface Guidelines - System Capabilities - Notifications](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/notifications/)

# 알림*Notifications*

애플리케이션은 디바이스가 잠겨있든 사용중이든, 언제든지 시기 적절하고 중요한 정보를 제공하기 위해 알림을 사용할 수 있습니다. 예를 들어, 알림은 메세지가 도착했을 때, 이벤트가 일어나려고 할 때, 새로운 데이터가 사용 가능할 때 발생할 수 있습니다. 사람들은 알림을 잠금 화면에서, 디바이스를 사용중일 때 화면 상단에서, 그리고 화면의 상단 모서리에서 아래로 스와이프하여 열리는 알림 센터*Notification Center*에서 확인합니다. 각각의 알림은 애플리케이션 이름, 작은 애플리케이션 아이콘, 그리고 메세지를 포함합니다. 알림은 또한 소리를 수반할 수 있으며, 상응하는 애플리케이션 아이콘에 뱃지가 나타나게 하거나 갱신하는 것을 유발할 수도 있습니다.

**참고.** 알림은 로컬 또는 원격에서 일어날 수 있습니다. 로컬 알림은 같은 디바이스에서 기원되어 전달됩니다. 할일 목록 애플리케이션은 다가오는 미팅이나 마감 날짜에 대해 누군가에게 알리기 위해 로컬 알림을 사용할 수 있습니다. 푸시 알림이라고도 불리는 원격 알림은 서버로부터 옵니다. 다중 사용자 게임은 플레이어들의 이동을 알 수 있도록 원격 알림을 사용할 수 있습니다.

## 알림 행동*Notification Behavior*

알림의 행동은 애플리케이션 별 기준으로 설정 앱에서 관리됩니다. 알림을 지원하는 어느 애플리케이션에서, 그 기능 전체를 활성화하거나 비활성화할 수 있습니다. 또한 알림 센터와 잠금 화면에서의 가시성을 활성화할 수 있고, 애플리케이션 아이콘에 뱃지를 다는 것을 활성화할 수 있고, 이러한 알림 스타일 중 하나를 선택할 수 있습니다.

- 배너. 디바이스가 사용중일 때 몇 초 동안 화면 상단에 나타난 후 사라짐
- 얼러트. 디바이스가 사용중일 화면 상단에 나타나 수동으로 사라질 때까지 상태를 유지함

디바이스가 잠금 해제되어있을 때 알림을 탭하거나, 디바이스가 잠겨있을 때 가장자리에서 스와이프하여, 알림을 사라지게 하고, 알림 센터에서 제거하고, 상응하는 애플리케이션을 열고, 관련된 정보를 표시합니다. 예를 들어, 잠금 해제된 디바이스에서 새로운 이메일 알림을 탭하는 것은 메일 앱을 열어 새로운 메세지를 표시합니다.

잠금 해제된 디바이스에서, 알림을 위로 스와이프하거나 사라지게 두는 것은 알림을 사라지게 하고 알림 센터에서 그것을 제거할 수도 있습니다.

잠금 해제된 디바이스 3D 터치를 사용하여 알림에 압력을 가하거나 알림을 아래로 스와이프하는 것은 확장된 세부 뷰를 엽니다. 이 뷰는 커스터마이징이 가능하고 액션을 취하기 위한 최대 네 개의 버튼을 포함할 수 있습니다. 예를 들어, 할일 목록 애플리케이션은 작업을 연기하고 작업을 완료된 것으로 표시하기 위한 액션을 포함하는 세부 뷰와 함께 작업 알림을 전달할 수 있습니다. 캘린더 앱의 이벤트 알림은 이벤트의 알람을 간단하게 지연시키는 Snooze 버튼을 제공합니다.

**참고.** 사람들은 알림을 지원하는 모든 애플리케이션에서 알림을 수신하는 것을 명시적으로 선택해야 합니다. 처음 애플리케이션을 사용할 때 그렇게 하도록 요청을 받습니다. 누군가가 그것에서 빠졌을 경우 다시 들어가기 위해 항상 설정 앱을 방문할 수 있습니다.

## 훌륭한 알림 경험 디자인하기*Designing a Great Notification Experience*

### 유용하고 유익한 알림을 제공하십시오.

사람들은 빠른 업데이트를 얻기 위해 알림을 활성화하므로, 값어치 있는 정보를 제공하는 데 집중하십시오. 문장의 경우, 적잘한 구두법을 사용하여 완전한 문장을 사용하고, 메세지를 자르지 마십시오. 시스템은 필요하다면 이것을 자동으로 행합니다. 사람들에게 애플리케이션을 열라고 말하지 말고, 특정 화면으로 탐색하고, 특정 버튼을 탭하고, 일단 알림이 사라지면 기억하기 힘든 다른 작업을 수행하십시오.

### 같은 것에 대해서 여러 번 알림을 보내지 마십시오. 심지어 사용자가 응답하지 않았더라 해도 말입니다.

사람들은 편의상 알림에 참석합니다. 같은 것에 여러 번의 알림을 전송하한다면, 알림 센터를 채워나갈 것이고, 사용자는 애플리케이션에서 알림 기능을 끌 것입니다.

### 애플리케이션 이름이나 아이콘을 포함하지 마십시오.

시스템이 각각의 알림의 상단에 이 정보를 자동으로 표시해 줍니다.

### 알림 미리보기가 숨겨졌을 때 표시하기 위한 설명 텍스트를 제공하십시오.

사용자의 설정에 기반하여, 알림 미리보기는 프라이버시를 위해 숨겨질 수 있습니다. 이 경우, 오직 애플리케이션 아이콘과 일반적인 설명 ("1개의 알림*Notification*"이 기본 설명)만 보여집니다. 사용자에게 충분한 컨텍스트를 제공하기 위해, 애플리케이션은 알림 컨텐츠를 간결하게 묘사하는, 친구 요청*Friend Request*, 새로운 댓글*New Comment*, 미리 알림*Reminder*, 출하*Shipment*와 같은 커스텀 텍스트를 제공해야 합니다. 개발자 가이드에서, [hiddenPreviewsBodyPlaceholder](https://developer.apple.com/documentation/usernotifications/unnotificationcategory/2873736-hiddenpreviewsbodyplaceholder)를 참고하십시오.

### 알림을 보완하기 위한 소리를 제공하십시오.

소리는 화면을 보고 있지 않을 때 누군가의 주의를 끌 수 있는 훌륭한 방법입니다. 예를 들어, 할일 목록 애플리케이션은 중요한 작업을 수행할 시간일 때 알림 소리를 재생할 수 있을 것입니다. 애플리케이션은 이것을 위해 커스텀 소리나 내장 알림 소리를 사용할 수 있습니다. 커스텀 소리를 사용한다면, 그것이 짧고, 독특하고, 전문적으로 생산되었는지를 확실하게 하십시오. [로컬 및 원격 알림 프로그래밍](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)에 있는 [커스텀 알림 소리 준비하기](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html#//apple_ref/doc/uid/TP40008194-CH4-SW10)를 참고하십시오. 사람들은 선택적으로 알림 소리를 비활성화할 수 있음을 염두에 두십시오. 그들은 또한 소리와 연관되어 있는 진동을 활성화할 수 있습니다. 이것은 수동으로 활성화되어야 하며 애플리케이션에서 프로그래밍적으로 활성화시킬 수 없습니다.

### 세부 뷰를 제공하는 것을 고려하십시오.

알림 세부 뷰는 알림에 대한 더 많은 정보를 제공하는데, 뿐만 아니라 애플리케이션을 열기 위해 현재 컨텍스트를 벗어나지 않고도 즉각적인 액션을 취할 수 있는 능력을 제공합니다. 이 뷰는 유용한 정보를 포함하고, 인식 가능해야 하고, 애플리케이션의 자연적인 확장인 것처럼 느껴져야 합니다. 사진, 비디오, 다른 컨텐츠를 포함할 수 있으며, 표시되는 동안 동적으로 갱신될 수 있습니다. 예를 들어, 카풀 애플리케이션은 당신의 현재 위치에 접근하고 있는 자동차를 보여주는 지도를 세부 뷰에 표시할 수 있을 것입니다.

### 직관적이고 유익한 액션을 제공하십시오.

알림 세부 뷰는 최대 네 개의 액션 버튼을 포함할 수 있습니다. 이 버튼들은 애플리케이션을 여는 필요를 제거하는 일반적이고, 시간을 아끼는 작업을 수행하는 것들이어야 합니다. 액션의 결과를 명백하게 설명하는 짧고, 제목 같은 이름을 사용하십시오. 알림 세부 뷰는 또한 액션을 취하기 위해 필요한 정보를 수집하기 위한 키보드를 화면 상에 표시할 수도 있습니다. 예를 들어, 메세지 애플리케이션은 새로운 메세지 알림으로부터 직접적으로 응답하게 할 수 있습니다.

### 파괴적인 액션을 제공하지 마십시오.

알림 세부 뷰에서 파괴적인 액션을 제공하기 전 주의 깊게 생각하십시오. 그것들을 제공해야 한다면, 의도하지 않은 결과를 피하기 위해 충분한 컨텍스트를 가지고 있는지를 확실하게 하십시오. 파괴적인 것으로 식별된 액션은 빨갛게 보입니다.

## 뱃지 달기*Badging*

### 중요한 정보를 나타내는 것이 아닌, 알림을 보완하기 위해 뱃지를 사용하십시오.

애플리케이션에 뱃지를 다는 기능은 꺼질 수 있음을 염두에 두십시오. 애플리케이션이 중요한 정보를 소통하는 것을 뱃지에 의존한다면, 사람들이 그것을 놓칠 위험을 안고 가는 것입니다.

### 오직 알림의 목적으로만 뱃지를 사용하십시오.

뱃지는 대기질, 날짜, 주식 가격, 또는 날씨와 같은 다른 종류의 수치 정보를 표시하는데 사용되어서는 안됩니다.

### 뱃지를 최신 상태로 유지하십시오.

상응하는 정보가 확인되자마자 뱃지를 갱신하십시오. 사람들이 새로운 정보가 사용 가능하다고 생각하게 하는 것을 원치 않으며, 결국 그들은 이미 그것을 확인했음을 알게 될 것입니다. 뱃지의 카운트를 0으로 줄이는 것은 알림 센터에 있는 모든 관련 알림을 제거한 것임을 기억하십시오.

## 더 알아보기*Learn More*

개발자 가이드에서, [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)를 참고하십시오.
