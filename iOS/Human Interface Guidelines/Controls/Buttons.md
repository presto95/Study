[Human Interface Guidelines - Controls - Buttons](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)

# 버튼*Buttons*

버튼은 애플리케이션에 특화된 액션을 개시하고, 커스터마이징 가능한 배경을 가지며, 타이틀이나 아이콘을 포함할 수 있습니다. 시스템은 대부분의 유스케이스를 위해 사전에 정의된 여러 개의 버튼 스타일을 제공합니다. 또한 완전한 커스텀 버튼을 디자인할 수 있습니다.

개발자 가이드에서, [UIButton](https://developer.apple.com/documentation/uikit/uibutton)을 참고하십시오.

## 시스템 버튼*System Buttons*

시스템 버튼은 대개 내비게이션 바 및 툴바에서 나타나나, 어디서든 사용될 수 있습니다.

### 타이틀에 동사를 사용하십시오.

액션에 특화된 타이틀은 버튼이 대화형이며 그것을 탭할 때 무엇이 일어날지 말하는 것을 보여줍니다.

### 타이틀에 타이틀 케이스를 사용하십시오.

관사, 접속사 및 4자 이하의 전치사를 제외한 모든 단어를 대문자로 작성하십시오.

### 타이틀을 짧게 유지하십시오.

과도하게 긴 텍스트는 인터페이스를 복잡하게 할 수 있으며 작은 화면에서 잘릴 수 있습니다.

### 필요할 때만 경계선이나 배경을 추가하는 것을 고려하십시오.

기본적으로, 시스템 버튼은 경계선이나 배경을 가지고 있지 않습니다. 그러나, 몇몇 컨텐츠 영역에서, 경계선이나 배경은 상호 작용성을 나타내기 위해 필수적입니다. 전화 앱에서, 경계선이 있는 숫자키들은 전화를 하는 전통적인 모델을 강화하며, 전화 버튼의 배경은 누르기 쉬운, 눈을 사로잡는 목표를 제공합니다.

개발자 가이드에서, [UIButton](https://developer.apple.com/documentation/uikit/uibutton)의 [UIButtonTypeSystem](https://developer.apple.com/documentation/uikit/uibuttontype/uibuttontypesystem) 버튼 타입을 참고하십시오.

## 상세 공개 버튼*Detail Disclosure Buttons*

상세 공개 버튼은 화면 상의 특정 아이템과 관련 있는 추가적인 정보나 기능을 포함하는, 일반적으로 모달 뷰를 엽니다. 어떠한 타입의 뷰에서라도 그것들을 사용할 수 있을지라도, 상세 공개 버튼은 특정 행에 대한 정보에 접근하기 위해 테이블에서 흔히 사용됩니다.

### 테이블에서 적절하게 상세 공개 버튼을 사용하십시오.

상세 공개 버튼이 테이블 행에 나타날 때, 버튼을 누르는 것은 추가적인 정보를 보여줍니다. 다른 곳을 탭하는 것은 행을 선택하거나 애플리케이션에 정의된 행동의 결과를 가져옵니다. 추가적인 상세 정보를 보기 위해 전체 행을 탭하기를 원한다면, 상세 공개 버튼을 사용하지 마십시오. 대신, 갈매기(>) 모양으로 보이는 상세 공개 악세사리 컨트롤을 사용하십시오. [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)의 [UITableViewCellAccessoryType](https://developer.apple.com/documentation/uikit/uitableviewcellaccessorytype)을 참고하십시오.

개발자 가이드에서, [UIButton](https://developer.apple.com/documentation/uikit/uibutton)의 [UIButtonTypeDetailDisclosure](https://developer.apple.com/documentation/uikit/uibuttontype/uibuttontypedetaildisclosure) 버튼 타입을 참고하십시오.

## 정보 버튼*Info Buttons*

정보 버튼은 때때로 뷰의 뒤에 위치하여, 주변 뷰를 뒤집은 후 애플리케이션에 대한 상세 구성을 드러냅니다. 정보 버튼은 밝고 어두운 두 가지 스타일로 옵니다. 애플리케이션의 디자인에 가장 잘 어울리고 화면 상에서 잃어버리지 않게 하는 스타일을 고르십시오.

개발자 가이드에서, [UIButton](https://developer.apple.com/documentation/uikit/uibutton)의 [UIButtonTypeInfoLight](https://developer.apple.com/documentation/uikit/uibuttontype/uibuttontypeinfolight)와 [UIButtonTypeInfoDark](https://developer.apple.com/documentation/uikit/uibuttontype/uibuttontypeinfodark) 버튼 타입을 참고하십시오.

## 연락처 추가 버튼*Add Contact Buttons*

사용자는 연락처 추가 버튼을 눌러 존재하는 연락처 목록을 훑어보며 텍스트 필드나 다른 뷰에 하나를 선택하여 삽입하기 위해 탭할 수 있습니다. 예를 들어, 메일 앱에서 메세지의 받는 사람*To* 필드에 있는 연락처 추가 버튼을 탭하여 연락처 목록에서 수신자를 선택할 수 있습니다.

### 연락처 추가 버튼 외에도 키보드 입력을 허용하십시오.

연락처 추가 버튼은 연락처 정보를 타이핑하기 위해 대체가 아닌 대안을 제공합니다. 존재하는 연락처를 더하기 위해 바로 가기로서 제공하는 것은 좋지만, 키보드로도 연락처 정보를 입력할 수 있게 하십시오.

개발자 가이드에서, [UIButton](https://developer.apple.com/documentation/uikit/uibutton)의 [UIButtonTypeContactAdd](https://developer.apple.com/documentation/uikit/uibuttontype/uibuttontypecontactadd) 버튼 타입을 참고하십시오.