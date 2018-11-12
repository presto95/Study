[Human Interface Guidelines - Controls - Steppers](https://developer.apple.com/design/human-interface-guidelines/ios/controls/steppers/)

# 스테퍼*Steppers*

스테퍼는 증가하는 값을 증가시키거나 감소시키는 데 사용되는 두 개의 단편을 가진 컨트롤입니다. 기본적으로, 스테퍼의 한 단편은 더하기 기호를 표시하며 다른 단편은 빼기 기호를 표시합니다. 이러한 기호들은 바람직하다면 커스텀 이미지로 교체될 수 있습니다.

### 값이 스테퍼에 의해 명백하게 영향을 받도록 하십시오.

스테퍼 그 자체로는 어떠한 값을 표시하지 않으므로, 스테퍼를 사용할 때 어떤 값이 변화하는지 알게 하는 것을 확실하게 하십시오.

### 큰 값 변화 가능성이 있을 때 스테퍼를 사용하지 마십시오.

스테퍼는 몇 번의 탭을 필요로 하는 작은 변화를 만들 때 잘 동작합니다. 예를 들어, 인쇄 화면에서 복사본의 수를 설정하기 위해 스테퍼를 사용하는 것이 말이 되는데, 사람들은 좀처럼 이 설정을 크게  바꾸지 않기 때문입니다. 반면에, 페이지 범위를 선택하기 위해 스테퍼를 사용하는 것은 말이 되지 않는데, 이성적인 페이지 범위일 지라도 많은 탭을 요구할 수 있기 때문입니다.

개발자 가이드에서, [UIStepper](https://developer.apple.com/documentation/uikit/uistepper)를 참고하십시오.