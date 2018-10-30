[Human Interface Guidelines - Controls - Edit Menus](https://developer.apple.com/design/human-interface-guidelines/ios/controls/edit-menus/)

# 편집 메뉴*Edit Menus*

사람들은 텍스트 필드, 텍스트 뷰, 웹 뷰 또는 이미지 뷰에 있는 요소를 터치하고 그 상태를 유지하거나 두 번 탭하여 컨텐츠를 선택하고 복사 및 붙여넣기와 같은 편집 옵션을 드러낼 수 있습니다.

### 현재 컨텍스트에 적절한 명령을 보여주십시오.

기본적으로, 옵션은 오려두기*Cut*, 복사하기*Copy*, 붙여넣기*Paste*, 전체 선택*Select All*, 지우기*Delete* 명령을 포함합니다. 이러한 것들은 선택적으로 비활성화될 수 있습니다. 어느 것도 선택되되어 있지 않다면, 메뉴는 복사나 오려두기와 같은, 선택을 필요로 하는 옵션을 표시하면 안됩니다. 비슷하게, 무언가가 이미 선택되어 있다면, 메뉴는 선택*Select* 옵션을 표시하면 안됩니다.

### 필요하다면, 편집 옵션의 배치를 조정하십시오.

기본적으로, 메뉴는 사용 가능한 공간에 따라 삽입 지점 또는 선택의 위나 아래에 배치되며, 관련된 컨텐츠를 가리키는 포인터를 포함합니다. 메뉴의 형태를 바꿀 수 없을지라도, 그 배치는 구성 가능합니다. 중요한 컨텐츠나 인터페이스의 일부분을 가리지 못하게 할 수 있습니다.

### 편집 메뉴와 같은 기능을 하는 다른 컨트롤을 구현하지 마십시오.

어떠한 작업을 개시하는 여러 개의 방법을 제공하는 것은 일관되지 않은 사용자 경험을 가져오고 혼란으로 이끕니다. 예를 들어, 애플리케이션이 메뉴를 사용하여 컨텐츠를 복사하게 한다면, 복사 버튼을 또 구현하지 마십시오.

### 잠재적으로 유용한 편집 불가한 텍스트가 선택되고 복사되는 것을 허용하십시오.

사람들은 종종 이미지 레이블이나 소셜 미디어 상태와 같은 정적 컨텐츠를 이메일, 메모 또는 웹 검색에 추가하는 것을 원합니다.

### 버튼에 편집 옵션을 추가하지 마십시오.

이것을 한다면, 옵션을 드러내는 시도를 하는 사람들은 대신에 버튼을 활성화하는 것을 끝낼 것입니다.

### 편집 작업은 실행 취소가 가능하게 하십시오.

메뉴는 그 액션이 수행되기 전 승인을 요구하지 않습니다. 누군가가 작업이 수행된 후 마음을 바꿀 수 있기 때문에, 실행 취소 및 실행 복귀 지원을 항상 구현하십시오.

### 유용한 커스텀 명령들로 편집 옵션을 확장하십시오.

애플리케이션에 특화된 명령들을 제공하여 가치를 더할 수 있습니다. 표준 명령들처럼, 어떤 커스텀 명령어들은 선택된 텍스트가 객체에서 동작해야 합니다.

### 시스템이 제공하는 명령 다음에 커스텀 명령을 보여주십시오.

잘 알려지고 빈번하게 사용되는, 시스템이 제공하는 명령들과 함께 커스텀 명령들을 배치하지 마십시오.

### 커스텀 명령의 수를 최소화 하십시오.

사람들이 너무 많은 선택에 압도되지 않도록 하십시오.

### 커스텀 명령의 이름은 짧게 유지하십시오.

명령 이름은 수행될 액션을 간결하게 묘사하는 동사나 동사구가 되어야 합니다. 관사, 접속사 및 4자 이하의 전치사를 제외하고 모든 단어를 대문자로 시작하는 타이틀 스타일의 대문자 쓰기를 사용하십시오.

개발자 가이드에서, [iOS 텍스트 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html)에 있는 [복사, 오려두기 및 붙여넣기 작업](https://developer.apple.com/library/content/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/UsingCopy,Cut,andPasteOperations/UsingCopy,Cut,andPasteOperations.html#//apple_ref/doc/uid/TP40009542-CH11-SW1)과 [UIMenuController](https://developer.apple.com/documentation/uikit/uimenucontroller)를 참고하십시오.