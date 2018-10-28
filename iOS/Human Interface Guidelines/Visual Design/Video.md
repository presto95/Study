[Human Interface Guidelines - Visual Design - Video](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/video/)

# 비디오*Video*

시스템이 제공하는 비디오 플레이어는 두 가지 시청 모드를 제공합니다. 하나는 전체 화면(aspect fill)이고, 다른 하나는 화면에 맞추는 것(aspect)입니다. 기본적으로, 시스템은 비디오의 종횡비에 기반하여 시청 모드를 선택하고, 사용자는 재생 도중 모드를 전환할 수 있습니다. 개발자 가이드에서, [AVPlayerViewController](https://developer.apple.com/documentation/avkit/avplayerviewcontroller)를 참고하십시오.

- **전체 화면 (aspect-fill) 시청 모드** : 비디오는 디스플레이를 가득 채우기 위해 크기가 조정됩니다. 몇몇 가장자리의 잘림이 발생할 수 있습니다. 와이드 비디오 (2:1~2.40:1)를 위한 기본 시청 모드입니다. 개발자 가이드에서, [resizeAspectFill](https://developer.apple.com/documentation/avfoundation/avlayervideogravity/1385607-resizeaspectfill)을 참고하십시오.

- **화면 맞춤 (aspect) 시청 모드** : 전체 비디오는 화면 상에서 보여질 수 있습니다. 레터박싱*letterboxing* 또는 필러박싱*pillarboxing*이 발생할 수 있습니다. 표준 비디오 (4:3, 16:9, 최대 2:1)와 울트라 와이드 비디오 (2.40:1 이상)를 위한 기본 시청 모드입니다. 개발자 가이드에서, [resizeAspect](https://developer.apple.com/documentation/avfoundation/avlayervideogravity/1387116-resizeaspect)를 참고하십시오.

  > 레터박싱, 필러박싱 : 컨텐츠 표시 시 디바이스의 양옆 또는 위아래가 비어 있는 현상

  ### 커스텀 비디오 플레이어가 기대한 대로 동작하는 것을 확실하게 하십시오.

  전체 화면 디바이스에서 비디오 컨텐츠가 재생중일 때 기본적으로 디스플레이를 가득 채우는 것이 목표입니다. 하지만, 디스플레이를 채우는 것이 너무 많은 잘림의 결과를 가져온다면, 비디오는 스크린에 맞추기 위해 크기가 조정되어야 합니다. 또한 개인의 선호도에 기반하여 전체 화면 시청 모드와 화면 맞춤 시청 모드 사이 전환을 가능하게 해야 합니다. 개발자 가이드에서, [AVPlayerLayer](https://developer.apple.com/documentation/avfoundation/avplayerlayer)를 참고하십시오.

### 항상 비디오 컨텐츠를 본래의 종횡비로 표시하십시오.

비디오 컨텐츠가 특정 종횡비를 준수하기 위해 포함된 레터박스 또는 필러박스 패딩*padding*을 사용할 때, iOS는 사용자의 시청 모드 선택에 기반하여 비디오의 크기를 정확하게 조정하는 것을 못하게 합니다. 비디오 프레임에 포함된 패딩은 전체 화면 모드와 화면 맞춤 모드에서 더 작게 보이게 하는 것을 유발할 수 있습니다. 그것은 또한 iPad의 사진 내 사진*Picture in Picture* 모드처럼, 전체 화면 컨텍스트가 아닌, 가장자리-가장자리*edge-to-edge*에서 올바르게 표시되는 것을 못하게 합니다.