[Human Interface Guidelines - Views - Popovers](https://developer.apple.com/design/human-interface-guidelines/ios/views/popovers/)

# 팝오버*Popovers*

팝오버는 컨트롤이나 영역 내를 탭할 때 화면 상의 컨텐츠 위에 나타나는 일시적인 뷰입니다. 일반적으로, 팝오버는 그것이 나타난 위치를 가리키는 화살표를 포함합니다. 팝오버는 모달일 수 있거나 모달이 아닐 수 있습니다. 모달이 아닌 팝오버는 화면의 또다른 부분이나 팝오버에 있는 버튼을 탭하여 사라지게 됩니다. 모달 팝오버는 팝오버에 있는 취소 또는 다른 버튼을 탭하여 사라지게 됩니다.

팝오버는 큰 화면에서 가장 적절하며 내비게이션 바, 툴바, 탭 바, 테이블, 컬렉션, 이미지, 지도 및 커스텀 뷰를 포함한 어떠한 다양한 요소라도 포함할 수 있습니다. 팝오버가 보일 때, 다른 뷰와의 상호 작용은 일반적으로 팝오버가 사라질 때까지 비활성화됩니다. 화면 상에 있는 컨텐츠와 관련된 선택지나 정보를 표시하기 위해 팝오버를 사용하십시오. 예를 들어, 많은 iPad 애플리케이션은 액션 버튼을 누를 때 공유 옵션에 대한 팝오버를 표시합니다.

### iPhone에서 팝오버를 표시하지 마십시오.

일반적으로, 팝오버는 iPad 애플리케이션에서 사용되기 위해 예약되어야 합니다. iPhone 앱에서는, 팝오버 보다는, 전체 화면 모달 뷰에서 정보를 나타내어 모든 사용 가능한 화면 공간을 활용하십시오. 관련된 가이드에서, [모달리티*Modality*](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/modality/)를 참고하십시오.

### 오직 승인 및 지침과 관련하여 닫기*Close* 버튼을 사용하십시오.

취소*Cancel* 및 완료*Done*과 같은 닫기 버튼은 그것이 변화를 저장하거나 저장하지 않고 종료하는 것과 같은 명백함을 제공한다면 포함될 가치가 있습니다. 일반적으로, 팝오버는 그 존재가 더이상 필요 없을 때 자동으로 닫아져야 합니다. 대부분의 경우에, 팝오버는 누군가가 그 경계의 바깥을 탭하거나 팝오버에 있는 아이템을 선택할 때 닫아져야 합니다. 여러 개의 선택이 이루어졌다면, 팝오버는 누군가가 명시적으로 사라지게 하거나 그 경계 바깥을 탭할 때까지 열린 상태를 유지해야 합니다.

### 모달이 아닌 팝오버를 자동으로 닫을 때 항상 작업을 저장하십시오.

화면의 또다른 부분을 탭하여 의도치 않게 모달이 아닌 팝오버를 사라지게 하는 것은 쉽습니다. 누군가가 명시적인 취소 버튼을 탭했을 때만 작업을 버리십시오.

### 화면 상에서 팝오버를 적절하게 배치하십시오.

팝오버의 화살표는 그것이 드러나게 된 요소를 가능한 한 직접적으로 가리켜야 합니다. 팝오버는 화면 주위로 드래그될 수 없기 때문에, 사람들이 팝오버를 사용하는 동안 보는 것을 필요로 할 수 있는 필수적인 컨텐츠를 덮어서는 안됩니다. 팝오버는 그것을 보이기 위해 탭한 요소도 덮어서는 안됩니다.

### 한 번에 하나의 팝오버를 보여주십시오.

여러 개의 팝오버를 표시하는 것은 인터페이스를 어지럽게 하고 혼란을 유발합니다. 또다른 것으로부터 하나가 나타나는, 단계적*cascading* 또는 계층적 팝오버를 절대 표시하지 마십시오. 새로운 팝오버를 표시할 필요가 있다면, 먼저 열린 것을 닫으십시오.

### 팝오버 위에 또다른 뷰를 표시하지 마십시오.

얼러트를 제외하고, 어느 것도 팝오버 위에 표시되어서는 안됩니다.

### 가능할 때, 사용자가 한 번의 탭으로 하나의 팝오버를 닫고 또다른 것을 열 수 있게 하십시오.

여분의 탭을 피하는 것은 몇몇 다른 바 버튼 각각이 팝오버를 열 때 특히 바람직합니다.

### 팝오버를 너무 크게 만들지 마십시오.

팝오버는 전체 화면을 가져가서는 안됩니다. 그 컨텐츠를 표시하기에 충분할 만큼만 크게 하고 그것이 비롯된 위치를 가리키십시오. 시스템은 그것이 화면 상에서 적합한 것을 보장하기 위해 크기를 조절할 수 있다는 것을 염두에 두십시오.

### 커스텀 팝오버가 팝오버처럼 보이는 것을 확실하게 하십시오.

당신이 팝오버의 많은 시각적 양상을 커스터마이징 할 수 있을지라도, 팝오버처럼 인식되지 않는 디자인을 하지 마십시오. 팝오버는 표준 컨트롤과 뷰를 포함할 때 가장 잘 작동하는 경향이 있습니다.

### 팝오버의 크기를 변경할 때 부드러운 전환을 제공하십시오.

몇몇 팝오버는 같은 정보에 대해서 축소된 뷰와 확장된 뷰 모두를 제공합니다. 팝오버의 크기를 조절하고 싶다면, 새로운 팝오버가 이전의 것을 대체한다는 느낌을 주지 않기 위해 변경에 애니메이션을 추가하십시오.

개발자 가이드에서, [UIPopOverPresentationController](https://developer.apple.com/documentation/uikit/uipopoverpresentationcontroller)를 참고하십시오.

