[Human Interface Guidelines - Extensions - Photo Editing](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/photo-editing/)

# 사진 편집*Photo Editing*

사진 편집 확장은 사람들이 필터를 적용하거나 다른 변화를 만들어 사진 앱 내에서 사진 및 비디오를 수정할 수 있게 합니다. 편집된 것은 새로운 파일로 사진 앱에 항상 저장되어, 원본을 안전하게 보존합니다.

사진 편집 확장에 접근하기 위해, 사진은 편집 모드에 있어야 합니다. 편집 모드에 있는 동안, 툴바에 있는 확장 아이콘을 탭하여 사용 가능한 편집 확장의 액션 메뉴를 표시합니다. 하나를 선택하여 내비게이션 바를 포함한 모달 뷰에서 확장의 인터페이스를 표시합니다. 이 뷰를 사라지게 하여 편집을 승인하고 저장하거나, 취소하고 사진 앱으로 되돌아갑니다.

### 편집 취소를 승인 받으십시오.

사진이나 비디오를 편집하는 것은 시간이 오래 걸립니다. 누군가가 취소 버튼을 탭한다면, 즉시 변화들을 버리지 마십시오. 정말로 취소하기를 원하는지 승인받기 위해 요청하고, 취소 후 편집 사항들을 잃게 될 것이라고 알리십시오. 어떠한 편집도 아직 일어나지 않았다면 이 승인을 보여줄 필요는 없습니다.

### 커스텀 내비게이션 바를 제공하지 마십시오.

당신의 확장은 이미 내비게이션 바를 포함하는 모달 뷰에서 로드됩니다. 추가적인 내비게이션 바를 제공하는 것은 혼란스럽게 하고 편집 중인 컨텐츠에서 멀리 떨어진 공간을 차지합니다.

### 편집 사항을 미리 보게 하십시오.

어떻게 보일지 알 수 없다면 편집을 승인하는 것은 어렵습니다. 확장을 닫고 사진 앱으로 되돌아가기 전 그들의 작업 결과를 보여주게 하십시오.

### 당신의 사진 편집 확장 아이콘에 애플리케이션 아이콘을 사용하십시오.

이것은 확장이 사실 당신의 애플리케이션으로부터 제공되었다는 자신감을 불어넣어 줍니다.

개발자 가이드에서, [애플리케이션 확장 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/index.html)의 [사진 편집](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/Photos.html)을 확인하십시오.