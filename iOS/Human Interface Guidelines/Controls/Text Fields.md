[Human Interface Guidelines - Controls - Text Fields](https://developer.apple.com/design/human-interface-guidelines/ios/controls/text-fields/)

# 텍스트 필드*Text Fields*

텍스트 필드는 사용자가 그것을 탭할 때 자동으로 키보드를 끌어올리는, 한 줄의, 고정 높이 필드며, 종종 둥근 모서리를 갖습니다. 이메일 주소와 같은 작은 양의 정보를 요청하기 위해 텍스트 필드를 사용하십시오.

### 목적을 전달하는 것을 도와주기 위해 텍스트 필드에 힌트를 보여주십시오.

텍스트필드는 필드에 다른 텍스트가 없을 때, "이메일"이나 "패스워드" 같은 플레이스홀더 텍스트를 포함할 수 있습니다. 플레이스홀더 텍스트가 충분할 때 텍스트 필드를 묘사하기 위한 분리된 레이블을 사용하지 마십시오.

### 적절할 때 텍스트 필드의 우측 끝에 클리어*Clear* 버튼을 표시하십시오.

이 요소가 나타날 때, 그것을 탭하여 텍스트 필드의 컨텐츠를 깨끗하게 하여, 삭제*Delete* 키를 계속 탭하고 있는 필요를 제거합니다.

### 적절할 때 안전한 텍스트 필드를 사용하십시오.

패스워드와 같은 민감한 데이터를 요청할 때 항상 안전한 텍스트 필드를 사용하십시오.

### 텍스트 필드에 뚜렷함과 기능성을 제공하기 위해 이미지 및 버튼을 사용하십시오.

텍스트 필드의 좌측 또는 우측 가장자리에 커스텀 이미지를 표시하거나, 북마크 버튼과 같은 시스템이 제공하는 버튼을 추가할 수 있습니다. 일반적으로, 텍스트 필드의 좌측 끝을 사용하여 필드의 목적을 가리키고, 우측 끝을 사용하여 북마킹과 같은 추가적인 기능의 존재를 가리킵니다.

개발자 가이드에서, [UITextField](https://developer.apple.com/documentation/uikit/uitextfield)를 참고하십시오.

**참고.** 여러 개의 행 또는 여러 개의 스타일을 가진 텍스트 입력을 위해, 텍스트 뷰를 대신 사용하십시오. [텍스트 뷰](https://developer.apple.com/design/human-interface-guidelines/ios/views/text-views)를 참고하십시오.

## 키보드*Keyboard*

### 적절한 키보드 타입을 보여주십시오.

iOS는 몇 가지 다른 키보드 타입을 제공하며, 각각은 다른 타입의 입력을 촉진하기 위해 디자인 되었습니다. 데이터 입력을 능률적으로 하기 위해, 편집 중인 텍스트 필드가 그 필드에 적절한 타입의 컨텐츠를 가져야 할 때 키보드가 표시됩니다. 예를 들어, 애플리케이션이 이메일 주소를 요청할 때, 이메일 주소 키보드를 표시해야 합니다. 사용 가능한 전체 키보드 타입 목록을 위해, [UITextInputTraits](https://developer.apple.com/documentation/uikit/uitextinputtraits)의 [UIKeyboardType](https://developer.apple.com/documentation/uikit/uikeyboardtype) 상수를 참고하십시오.

관련된 가이드를 위해, [커스텀 키보드](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/custom-keyboards/)를 참고하십시오.