[Human Interface Guidelines - Views - Image Views](https://developer.apple.com/design/human-interface-guidelines/ios/views/image-views/)

# 이미지 뷰*Image Views*

이미지 뷰는 단일 이미지 또는 투명 또는 불투명한 배경 위에서 생동감 잇는 이미지 시퀀스를 표시합니다. 이미지 뷰 내에서, 이미지는 늘려질 수 있고, 크기가 조정될 수 있고, 딱 맞게 조절될 수 있거나, 특정 위치에 고정될 수 있습니다. 이미지 뷰는 기본적으로 상호 작용 가능하지 않습니다.

### 가능하다면, 애니메이션 시퀀스에 있는 모든 이미지가 일관된 크기를 갖는 것을 확실하게 하십시오.

이상적으로, 이미지는 뷰에 알맞게 미리 크기가 조정되어 시스템이 어떠한 스케일링 작업을 하지 않도록 해야 합니다. 시스템이 스케일링을 수행해야 한다면, 모든 이미지가 같은 크기와 형태를 할 때 바라는 결과를 얻기 가장 쉽습니다.

개발자 가이드에서, [UIImageView](https://developer.apple.com/documentation/uikit/uiimageview)를 참고하십시오.

**참고.** 템플릿 이미지로 구성된 이미지는 그 색상을 버리고 그것을 감싸는 이미지 뷰에 적용된 어떠한 틴트 색상을 채택합니다. [커스텀 아이콘](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/)을 참고하십시오. 개발자 가이드에서, [UIImage](https://developer.apple.com/documentation/uikit/uiimage)의 [UIImageRenderingModeAlwaysTemplate](https://developer.apple.com/documentation/uikit/uiimagerenderingmode/uiimagerenderingmodealwaystemplate)를 참고하십시오.