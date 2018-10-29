[Human Interface Guidelines - Views - Action Sheets](https://developer.apple.com/design/human-interface-guidelines/ios/views/action-sheets/)

# 액션 시트*Action Sheets*

액션 시트는 컨트롤이나 액션의 응답으로 나타나는 얼러트의 특정 스타일이며, 현재 컨텍스트와 관련된 두 개 이상의 선택 집합을 나타냅니다. 사람들이 작업을 개시하거나, 잠재적으로 파괴적인 동작을 수행하기 전 승인을 요청하기 위해 액션 시트를 사용하십시오. 작은 화면에서, 액션 시트는 화면의 아래에서부터 슬라이드하여 올라옵니다. 큰 화면에서, 액션 시트는 팝오버처럼 모두 한번에 나타납니다.

### 뚜렷함을 더한다면 취소 버튼을 제공하십시오.

취소 버튼은 사용자가 작업을 버릴 때 자신감을 불어넣어 줍니다. 취소 버튼은 화면 하단의 액션 시트에 항상 포함되어야 합니다.

### 파괴적인 선택을 현저하게 하십시오.

파괴적이거나 위험한 액션을 수행하는 버튼을 빨갛게 하고, 이러한 버튼을 액션 시트의 상단에 표시하십시오.

### 액션 시트에서 스크롤을 가능하게 하지 마십시오.

액션 시트가 너무 많은 선택지를 갖는다면, 모든 선택지를 보기 위해 스크롤 해야만 합니다. 스크롤은 선택을 하는 데 여분의 시간을 필요로 하며 우연하게 버튼을 탭하지 않고서 어떤 것을 하기 어렵습니다.

개발자 가이드에서, [UIAlertController](https://developer.apple.com/documentation/uikit/uialertcontroller)의 [UIAlertControllerStyleActionSheet](https://developer.apple.com/documentation/uikit/uialertcontrollerstyle/uialertcontrollerstyleactionsheet) 상수를 참고하십시오.