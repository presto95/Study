[Human Interface Guidelines - Extensions - Sharing and Actions](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/sharing-and-actions/)

# Sharing and Actions*공유 및 액션*

공유 확장은 애플리케이션, 소셜 미디어 계정 및 다른 서비스와 현재 컨텍스트에서 정보를 공유하는 편리한 방법을 제공합니다. 액션 확장은 북마크 추가, 링크 복사 또는 이미지 저장과 같은 컨텐츠에 특화된 작업을 개시하게 합니다. 사람들은 액티비티 뷰를 표시하기 위해 애플리케이션 안에 있는 액션 버튼을 탭하여 공유 및 액션 확장에 접근합니다. 액티비티 뷰는 오직 확장이 현재 컨텍스트와 관련되어 있을 때만 보여집니다. 예를 들어, 비디오를 편집중일 때 텍스트 조절 액션을 보게 해서는 안됩니다. 액티비티 뷰 내에서, 공유 확장은 액션 확장 위에 목록으로 나타납니다.

### 단일의, 집중된 작업을 가능하게 하십시오.

확장은 미니 앱이 아닙니다. 현재 컨텍스트에 관련된 좁은 범위의 작업을 수행할 뿐입니다.

### 익숙한 인터페이스를 만드십시오.

공유 익스텐션에서, 시스템이 제공하는 구성 뷰는 익숙하며 시스템 도처에서 일관된 공유 경험을 제공합니다. 가능할 때마다 그것을 사용하십시오. 액션 확장에서, 애플리케이션 이름을 포함하거나, 인식 가능하고 애플리케이션의 자연적인 확장인 것처럼 느끼게 하는 인터페이스를 디자인 하십시오.

### 상호 작용을 간소화하고 제한하십시오.

최상의 확장은 단지 몇 단계 만으로 작업을 수행하게 합니다. 예를 들어, 공유 확장은 한 번의 탭으로 소셜 미디어 계정에 이미지를 즉시 포스트할 것입니다. 필요할 때만 인터페이스를 제공하십시오.

### 확장에 모달 뷰를 배치하지 마십시오.

확장은 기본적으로 모달 뷰 내에서 표시됩니다. 얼러트가 확장 위에 있는 것이 말이 될지라도, 추가적인 모달 뷰를 올리지 마십시오.

### 연장 작업의 진전을 나타내기 위해 메인 애플리케이션을 사용하십시오.

액티비티 뷰는 공유나 액션을 개시한 후 즉시 사라져야 합니다. 시간이 걸리는 작업은 백그라운드에서 지속되어야 하며, 메인 애플리케이션은 이러한 작업의 상태를 확인할 수 있는 몇 가지 방법을 제공해야 합니다. 이것에 알림을 사용하지 마십시오. 문제가 있다면 알림을 주는 것은 좋을지라도, 사람들은 작업이 완료될 때마다 알림을 보는 것을 원하지 않습니다.

### 액션 확장 아이콘을 위해 템플릿 이미지를 사용하십시오.

템플릿 이미지는 아이콘을 만들기 위해 마스크를 사용합니다. 적절한 투명도를 갖고 안티에일리어싱 처리를 했으며, 그림자를 포함하지 않는, 검은색과 흰색을 사용하십시오. 템플릿 이미지는 약 70px x 70px로 측정되는 영역의 중앙에 위치해야 합니다.

추가적인 가이드를 위해, [액티비티 뷰](https://developer.apple.com/design/human-interface-guidelines/ios/views/activity-views/)를 참고하십시오. 개발자 가이드에서, [애플리케이션 확장 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/index.html)의 [공유](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/Share.html) 및 [액션](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/Action.html)을 참고하십시오.

**참고.** 공유 익스텐션은 자동으로 애플리케이션 아이콘을 사용하여, 확장이 사실은 애플리케이션에 의해 제공되었다는 자신감을 불어넣어 줍니다.