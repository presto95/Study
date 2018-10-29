[Human Interface Guidelines - Views - Text Views](https://developer.apple.com/design/human-interface-guidelines/ios/views/text-views/)

# 텍스트 뷰*Text Views*

텍스트 뷰는 여러 줄의, 스타일 있는 텍스트 컨텐츠를 표시합니다. 텍스트 뷰는 어떠한 높이라도 가질 수 있으며 컨텐츠가 뷰의 바깥으로 연장될 때 스크롤을 활성화합니다. 기본적으로, 텍스트 뷰 내에 있는 컨텐츠는 좌측 정렬되어 있으며 검은색 시스템 폰트를 사용합니다. 텍스트 뷰가 편집 가능하다면, 뷰 안을 탭할 때 키보드가 나타납니다.

### 텍스트를 읽기 쉽게 유지하십시오.

창조적인 방식으로 다양한 글꼴, 색상 및 정렬을 사용할 수 있을지라도, 컨텐츠의 가독성을 유지하는 것이 필수적입니다. 동적 타입*Dynamic Type*을 채택하여 사람들이 디바이스의 텍스트 크기를 변경할지라도 여전히 좋게 보이게 하는 것은 좋은 생각입니다. 또한 볼드체 텍스트*bold text*와 같은 손쉬운 사용 옵션이 활성화된 것으로 컨텐츠를 테스트해야 합니다.

### 적절한 키보드 타입을 보여주십시오.

iOS는 몇 가지 다른 키보드 타입을 제공하고, 각각은 다른 타입의 입력을 촉진하기 위해 디자인 되었습니다. 데이터 입력을 능률적으로 하기 위하여, 텍스트 뷰의 편집 동안 표시되는 키보드는 그 필드의 컨텐츠 타입에 적절해야 합니다. 사용 가능한 키보드 타입의 완전한 목록을 위해, [UITextInputTraits](https://developer.apple.com/documentation/uikit/uitextinputtraits)의 [UIKeyboardType](https://developer.apple.com/documentation/uikit/uikeyboardtype) 상수를 참고하십시오.

개발자 가이드에서, [UITextView](https://developer.apple.com/documentation/uikit/uitextview)를 참고하십시오.