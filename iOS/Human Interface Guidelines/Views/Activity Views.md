[Human Interface Guidelines - Views - Activity Views](https://developer.apple.com/design/human-interface-guidelines/ios/views/activity-views/)

# 액티비티 뷰*Activity Views*

액티비티는 복사, 즐겨찾기 또는 찾기와 같은, 현재 컨텍스트에서 유용한 작업입니다. 일단 개시되면 액티비티는 즉시 어떤 작업을 수행할 수 있거나, 진행되기 전 더 많은 정보를 요청할 수 있습니다. 액티비티는 액티비티 뷰에 의해 관리되며, 디바이스와 방향에 따라 시트나 팝오버의 형태로 나타납니다. 액티비티를 사용하여 사람들에게 커스텀 서비스나 애플리케이션이 수행할 수 있는 작업에 접근하게 하십시오.

시스템은 인쇄, 메세지, AirPlay를 포함하는 몇 가지의 내장 액티비티를 제공합니다. 이러한 작업은 항상 액티비티 뷰에 첫 번째로 나타나며 다시 정렬할 수 없습니다. 이러한 내장 작업을 수행하는 커스텀 액티비티를 만들 필요는 없습니다. 액티비티 뷰는 또한 다른 애플리케이션에서 온 공유 및 액션 확장을 표시합니다. [공유 및 액션](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/sharing-and-actions)을 참고하십시오.

### 당신의 커스텀 액티비티를 나타내기 위한 간단한 템플릿 이미지를 디자인 하십시오.

템플릿 이미지는 아이콘을 만들기 위해 마스크를 사용합니다. 적절한 투명도와 안티에일리어싱 처리가 된 검고 흰 색상을 사용하며, 그림자는 포함하지 마십시오. 템플릿 이미지는 약 70px x 70px로 측정되는 영역에서 중앙에 위치해야 합니다.

### 작업을 간단하게 묘사하는 액티비티 타이틀을 만드십시오.

타이틀은 액티비티 뷰의 아이콘 아래에 나타납니다. 짧은 타이틀이 가장 좋습니다. 타이틀이 너무 길 때, iOS는 먼저 텍스트를 수축시키고, 여전히 너무 길다면, 자릅니다. 일반적으로, 타이틀에 회사 및 제품 이름을 포함하지 마십시오.

### 액티비티가 현재 컨텍스트에서 적절하다는 것을 확실하게 하십시오.

시스템이 제공하는 작업이 액티비티에서 다시 정렬될 수 없다고 할지라도, 그것들이 애플리케이션에서 적용 가능하지 않다면 제외될 수 있습니다. 예를 들어, 사람들이 이미지를 출력하는 것을 막기 위해, 인쇄 액티비티를 제외할 수 있습니다. 또한 주어진 시간에 무슨 커스텀 작업을 표시할지 식별할 수 있습니다.

### 액티비티 뷰를 표시하기 위해 액션*Action* 버튼을 사용하십시오.

사람들은 액션 버튼을 누를 때 시스템이 제공하는 액티비티에 접근하는 것에 익숙해져 있습니다. 같은 것을 하기 위한 대안의 방법을 제공하여 사람들을 혼란스럽게 하지 마십시오.

개발자 가이드에서, [UIActivityViewController](https://developer.apple.com/documentation/uikit/uiactivityviewcontroller)와 [UIActivity](https://developer.apple.com/documentation/uikit/uiactivity)를 참고하십시오.