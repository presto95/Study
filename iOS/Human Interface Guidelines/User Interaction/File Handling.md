[Human Interface Guidelines - User Interaction - File Handling](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/file-handling/)

# 파일 다루기*File Handling*

사람들은 파일을 생성하고, 보고, 다루는 동안 파일 시스템에 대해 생각할 필요가 없어야 합니다. 애플리케이션이 파일과 함께 동작한다면, 가능한 한 파일 처리를 경시*downplay*하십시오.

### 작업이 취소되거나 삭제되지 않는 한 항상 보존되어야 한다는 확신을 서서히 불어넣으십시오.

일반적으로, 사람들이 명시적으로 파일을 저장하게 하지 마십시오. 대신, 규칙적인 간격에, 파일을 열고 닫을 때, 또다른 애플리케이션으로 전환할 때 변화를 자동으로 저장하십시오. 몇몇 경우에, 말하자면 존재하는 파일을 편집하는 중에, 편집한 것들을 실제로 캡처한 때를 확인하기 위한 저장 및 취소 옵션은 여전히 말이 됩니다.

### 로컬 전용 파일을 생성하는 옵션을 제공하지 마십시오.

사용자는 종종 그들의 파일 모두가 그들의 모든 디바이스에서 이용 가능할 것이라고 기대합니다. 가능할 때마다, 애플리케이션은 iCloud와 같은 서비스를 통해 클라우드 기반 파일 저장소를 지원해야 합니다.

### 직관적이고 그래픽적인 파일 탐색 인터페이스를 구현하십시오.

이상적으로, 탐색을 위해 시스템의 잘 알려진 문서 피커를 사용하십시오. 커스텀 파일 탐색기를 구현한다면, 그것이 직관적이고 효율적인 것을 확실하게 하십시오. 파일 탐색기는 그것이 그래픽적으로 수준이 높아, 파일의 시각적 표현을 제공할 때 가장 잘 작동합니다. 더 빠른 탐색을 위해, 새로운 문서*new document* 버튼을 제공하여 새로운 문서를 생성하기 위해 다른 곳으로 갈 필요가 없게 하는 것을 고려하십시오.

### 사용자들이 애플리케이션을 떠나지 않고 파일을 미리 볼 수 있게 하십시오.

사람들이 Keynote, Numbers, Pages의 문서들, PDF, 이미지, 그리고 특정한 다른 타입의 파일의 컨텐츠를, 애플리케이션이 실제고 그것들을 열지 않더라도, 훑어보기*Quick Look*를 사용하여 볼 수 있게 할 수 있습니다. [훑어보기](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/quick-look/)를 참고하십시오.

### 적절한 때에, 다른 애플리케이션과 파일을 공유하십시오.

말이 된다면, 애플리케이션은 [문서 제공자 확장*document provider extension*](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/document-providers/)을 통해 다른 애플리케이션과 파일을 공유할 수 있습니다. 애플리케이션은 또한 사람들이 다른 애플리케이션에서 파일을 탐색하고 열 수 있게 할 수 있습니다. 개발자 가이드에서, [문서 피커*Document Picker* 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/DocumentPickerProgrammingGuide/Introduction/Introduction.html)를 참고하십시오.

