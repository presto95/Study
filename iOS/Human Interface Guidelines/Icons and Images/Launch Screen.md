[Human Interface Guidelines - Icons and Images - Launch Screen](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/launch-screen//)

# 런치스크린*Launch Screen*

런치스크린은 애플리케이션이 시작하자마자 나타납니다. 런치스크린은 애플리케이션의 맨 처음 화면으로 빠르게 교체되며, 애플리케이션이 빠르고 반응적이라는 인상을 줍니다. 런치스크린은 예술적 표현을 위한 기회가 아닙니다. 그것은 오로지 애플리케이션이 빠르게 실행되고 즉시 사용 준비가 된다는 관점을 향상시키는 것을 지향합니다. 모든 애플리케이션은 런치스크린을 제공해야 합니다.

디바이스의 화면 크기는 다양하기 때문에, 런치스크린의 크기 또한 다양합니다. 이것에 적응하기 위해, Xcode의 스토리보드 또는 애플리케이션이 지원하는 디바이스를 위한 정적 이미지의 집합으로 런치스크린을 제공해야 합니다. Xcode의 스토리보드를 사용하는 것이 추천되는 접근법인데, 스토리보드는 유연하고 반응적이기 때문입니다. 모든 런치스크린을 관리하기 위해 하나의 스토리보드를 사용해야 합니다. 반응형 인터페이스르 구현하는 것에 대해 더 알아보기 위해, [오토레이아웃 가이드](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)를 참고하십시오.

### 애플리케이셔의 첫 번째 화면과 거의 동일한 런치스크린을 디자인하십시오.

애플리케이션이 실행 준비를 마쳤을 때 다르게 보이는 요소를 포함한다면, 사람들은 런치스크린과 첫 번째 화면 사이의 불쾌한 섬광*flash*를 경험할 수 있습니다.

### 런치스크린에 텍스트를 포함하지 마십시오.

런치스크린은 정적이기 때문에, 표시되는 어떠한 텍스트도 지역화되지 않을 것입니다.

### 실행을 중시하지 마십시오.*Downplay launch.*

사람들은 주기적으로 애플리케이션을 전환할 것이므로, 애플리케이션 실행 경험에 주의를 끌게 하지 않는 런치스크린을 디자인 하십시오.

### 광고하지 마십시오.

런치스크린은 브랜딩의 기회가 아닙니다. 스플래시 화면처럼 보이거나 "About" 윈도우처럼 보이는 엔트리 경험을 디자인하지 마십시오. 그것들이 애플리케이션의 첫 번째 화면의 정적인 부분이 아니라면, 로고나 다른 브랜딩 요소를 포함하지 마십시오.

## 정적 런치스크린 이미지*Static Launch Screen Images*

런치스크린을 위해 Xcode의 스토리보드를 사용하는 것이 가장 좋으나, 필요하다면 정적 이미지의 집합을 제공할 수 있습니다. 다른 디바이스를 위한 다른 크기의 정적 이미지를 만들고, 상태 바 영역을 포함하는 것을 확실하게 하십시오.

| 디바이스                                          | 세로 크기       | 가로 크기   |
| ------------------------------------------------- | --------------- | ----------- |
| 12.9인치 iPad Pro                                 | 2048px × 2732px | 세로와 반대 |
| 10.5인치 iPad Pro                                 | 1668px × 2224px |             |
| 나머지 iPad                                       | 1536px × 2048px |             |
| iPhone XS Max                                     | 1242px × 2688px |             |
| iPhone XR                                         | 828px × 1792px  |             |
| iPhone X / XS                                     | 1125px × 2436px |             |
| 5.5인치 iPhone (iPhone 8 Plus, iPhone 7 Plus...)  | 1242px × 2208px |             |
| 4.7인치 iPhone (iPhone 6S, iPhone 7, iPhone 8...) | 750px × 1334px  |             |
| 4인치 iPhone (iPhone SE...)                       | 640px × 1136px  |             |

