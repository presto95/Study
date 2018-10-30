[Human Interface Guidelines - Controls - Sliders](https://developer.apple.com/design/human-interface-guidelines/ios/controls/sliders/)

# 슬라이더*Sliders*

슬라이더는 썸브*thumb*라고 불리는 컨트롤이 있는 수평 트랙입니다. 당신은 손가락을 슬라이드하여 화면의 밝기 수준 또는 미디어가 재생 중인 위치와 같은, 최소 및 최대값 사이를 움직일 수 있습니다. 슬라이더의 값이 변화할 때, 최소값과 썸브 사이의 트랙 부분은 색상으로 채워집니다. 슬라이더는 선택적으로 최소값 및 최대값의 의미를 설명하는 좌측 및 우측 아이콘을 표시할 수 있습니다.

### 가치를 전달한다면 슬라이더의 생김새를 커스터마이징 하십시오.

트랙 색상, 썸브 이미지 및 좌측 및 우측 아이콘을 포함하는 슬라이더의 생김새는 이플리케이션의 디자인과 어울리고 의도를 전달하기 위해 조절될 수 있습니다. 예를 들어, 이미지 크기를 조절하는 슬라이더는 좌측에는 작은 이미지 아이콘을 표시하고 우측에는 큰 이미지 아이콘을 표시할 수 있을 것입니다.

### 오디오 볼륨을 조절하기 위해 슬라이더를 사용하지 마십시오.

애플리케이션에서 볼륨 컨트롤을 제공할 필요가 있다면, 커스터마이징 가능하고 볼륨 수준 슬라이더를 포함하는 볼륨 뷰와 활성화된 오디오 출력 디바이스를 변경하기 위한 컨트롤을 사용하십시오. 볼륨 뷰를 구현하는 것을 더 알아보기 위해, [MPVolumeSlider](https://developer.apple.com/documentation/mediaplayer/mpvolumeview)를 참고하십시오.

개발자 가이드에서, [UISlider](https://developer.apple.com/documentation/uikit/uislider)를 참고하십시오.