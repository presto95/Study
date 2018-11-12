[Human Interface Guidelines - User Interaction - Near Field Communication](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/gestures/)

# 근거리 무선통신*Near Field Communication*

근거리 무선통신(NFC)는 몇 센티미터 내에 있는 디바이스끼리 무선으로 정보를 교환하는 것을 가능하게 합니다. 지원되는 디바이스에서 작동하고 있는 iOS 애플리케이션은 실제 세계의 객체에 부착된 전자 태그로부터 데이터를 읽어들이기 위해 NFC 스캐닝을 사용할 수 있습니다. 예를 들어, 사용자는 장난감을 스캔하여 그것을 비디오 게임과 연결시킬 수 있거나, 쇼핑하는 사람은 쿠폰에 접근하기 위해 점내의 기호를 스캔할 수 있으며, 소매점 종업원은 재고를 추적하기 위해 제품을 스캔할 수 있습니다.

개발자 가이드에서, [코어 NFC*Core NFC*](https://developer.apple.com/documentation/corenfc)를 참고하십시오.

## 인앱 태그 읽기*In-App Tag Reading*

애플리케이션은 그 애플리케이션이 활성화되어 있을 때 단일 또는 다중 객체 스캐닝을 활성화할 수 있고, 사용자가 무언가를 스캔하려고 할 때마다 스캐닝 시트를 표시할 수 있습니다.

### 사람들이 물리 객체와 접촉할 것임을 장려하지 마십시오.

태그를 스캔하기 위해, iOS 디바이스는 태그의 바로 가까이에 있기만 하면 됩니다. 실제로 태그를 터치할 필요는 없습니다. 사람들에게 객체를 스캔할 것을 요청할 때, *탭*과 *터치* 대신 *스캔*과 *가까이 다가가기hold near*와 같은 용어를 사용하십시오.

### 이해하기 쉬운 용어를 사용하십시오.

NFC는 몇몇 사람들에게 친숙하지 않을 수 있습니다. 이것을 이해하기 쉽게 하기 위해, NFC, Core NFC, 근거리 무선통신, 태그와 같은 기술적이고, 개발자 지향적인 용어를 언급하지 마십시오. 대신, 대부분의 사람들이 이해할 수 있는 친숙하고 일상적인 용어를 사용하십시오.

| 사용                                                         | 사용 지양                                                 |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| [객체 이름]을 스캔하십시오.                                  | NFC 태그를 스캔하십시오.                                  |
| 더 많은 정보를 확인하려면 iPhone을 [객체 이름] 근처에 두십시오. | NFC 스캐닝을 사용하기 위해, 핸드폰을 [객체]에 탭하십시오. |

### 스캐닝 시트에 간결한 교육적 텍스트를 제공하십시오.

문장의 경우, 구두점으로 끝내는 완전한 문장을 사용하십시오. 스캔할 객체를 식별하고, 이어지는 스캔을 위해 적절한 텍스트로 수정하십시오. 끊어지지 않도록 텍스트를 짧게 유지하십시오.

| 첫 번째 스캔                                                 | 이어지는 스캔                                      |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 더 많은 정보를 확인하기 위해 iPhone을 [객체 이름] 근처에 두십시오. | 이제 iPhone을 또 다른 [객체 이름] 근처에 두십시오. |

## 백그라운드 태그 읽기*Background Tag Reading*

백그라운드 먼저 애플리케이션을 열고 스캐닝을 개시할 필요 없이 언제든지 태그 스캔을 빠르게 할 수 있도록 합니다. 백그라운드 태그 읽기를 지원하는 디바이스에서, 시스템은 화면이 밝아질 때마다 근처에 있는 호환되는 태그를 자동으로 찾습니다. 태그를 감지하여 애플리케이션과 일치시킨 후, 시스템은 태그 데이터를 처리하기 위해 애플리케이션으로 데이터를 보내기 위해 탭할 수 있다는 알림을 보여줍니다. 백그라운드 읽기는 NFC 스캐닝 시트가 보여지고 있고, Wallet 앱이나 Apple Pay가 사용중이고, 카메라가 사용중이고, 디바이스가 비행기 모드이고, 디바이스가 재시작 후 잠겨 있을 때는 비활성화되어 있음을 알아두십시오.

### 백그라운드 태그 읽기와 인앱 태그 읽기 모두를 지원하십시오.

애플리케이션은 여전히 태그를 스캔하기 위해 인앱에서의 방법을 제공해야 하는데, 백그라운드 태그 읽기를 지원하지 않는 디바이스를 가지고 있는 사람들을 위해서 그래야 합니다.