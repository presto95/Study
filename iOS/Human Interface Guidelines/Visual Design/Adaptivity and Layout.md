[Human Interface Guidelines - Visual Design - Adaptivity and Layout](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/)

# 적응성과 레이아웃*Adaptivity and Layout*

사람들은 일반적으로 그들의 모든 디바이스와 어떠한 컨텍스트 내에서라도 가장 좋아하는 애플리케이션을 사용할 수 있기를 원합니다. iOS에서, 인터페이스 요소와 레이아웃은 다른 디바이스에서, iPad의 멀티태스킹 도중 스플릿 뷰에서, 화면이 회전될 때, 그리고 더 많은 경우에 그 모양과 크기를 자동으로 변화합니다. 어떠한 환경에서도 좋은 경험을 제공하는 반응형 인터페이스를 디자인하는 것은 필수적입니다.

## 디바이스 화면 크기 및 방향*Device Screen Size and Orientations*

iOS 디바이스는 다양한 화면 크기로 제공되며 세로 또는 가로 방향으로 사용될 수 있습니다.

| 기기                                        | 세로 방향       | 가로 방향   |
| ------------------------------------------- | --------------- | ----------- |
| 12.9인치 iPad Pro                           | 2048px x 2732px | 세로와 반대 |
| 10.5인치 iPad Pro                           | 1668px x 2224px |             |
| 9.7인치 iPad                                | 1536px x 2048px |             |
| 7.9인치 iPad mini 4                         | 1536px x 2048px |             |
| iPhone XS Max                               | 1242px x 2688px |             |
| iPhone XS                                   | 1125px x 2436px |             |
| iPhone XR                                   | 828px x 1792px  |             |
| 5.5인치 iPhone (6S Plus, 7 Plus, 8 Plus 등) | 1242px x 2208px |             |
| 4.7인치 iPhone (6S, 7, 8 등)                | 750px x 1334px  |             |
| 4인치 iPhone (SE, 5S 등)                    | 640px x 1136px  |             |



애플리케이션의 아트워크에 화면 해상도가 어떻게 영향을 주는지 더 알아보기 위해, [이미지 크기 및 해상도](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)를 참고하십시오.

## 오토레이아웃*Auto Layout*

오토레이아웃은 반응형 인터페이스를 구축하기 위한 개발 도구입니다. 오토레이아웃을 사용하여, 애플리케이션 내의 컨텐츠를 지배하는, 제약*constraints*으로 알려진 규칙을 정의할 수 있습니다. 예를 들어, 이용 가능한 화면 공간에 상관 없이, 어떤 버튼이 항상 수평적으로 중앙에 위치하고 어떤 이미지에서 8포인트 아래에 위치하도록 할 수 있도록 강요할 수 있습니다.

오토레이아웃은 특징*traits*으로 알려진, 특정 환경 변수가 감지될 때 특정 제약에 따라 레이아웃을 자동으로 다시 조절합니다. 애플리케이션이 다음을 포함하는 넓은 범위의 특징을 동적으로 채택하게 할 수 있습니다.

- 다른 디바이스 [화면 크기](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#device-screen-sizes-and-orientations), [해상도](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/), 그리고 [색상 범위*color gamuts* (sRGB/P3)](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/(/design/human-interface-guidelines/ios/visual-design/color/#color-management))
- 다른 디바이스 방향 (세로 / 가로)
- [스플릿 뷰](https://developer.apple.com/design/human-interface-guidelines/ios/views/split-views/)
- iPad에서의 [멀티태스킹](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/multitasking/) 모드
- [동적 타입](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/) 텍스트 크기 변화
- 장소에 기반하여 활성화 가능한 국제화 기능 (왼쪽에서 오른쪽 / 오른쪽에서 왼쪽 레이아웃 방향, 날짜 / 시간 / 숫자 형식, 글꼴 변화, 텍스트 길이)
- 시스템 기능 사용 가능성 ([3D 터치](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/3d-touch/))

개발자 가이드에서, [오토레이아웃 가이드](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)와 [UITraitCollection](https://developer.apple.com/documentation/uikit/uitraitcollection)을 참고하십시오.

## 레이아웃 가이드와 안전 영역*Layout Guides and Safe Area*

레이아웃 가이드는 화면상에서 실제로 가시적으로 나타나면 안되는 직사각형 영역을 정의하지만, 컨텐츠의 배치, 정렬, 간격 설정에 도움을 줍니다. 시스템은 최적의 가독성을 위해 컨텐츠 주위에 표준 마진을 적용하고 텍스트의 너비를 제한하는 것을 쉽게 하는 사전에 정의된 레이아웃 가이드를 포함합니다.

### UIKit에 의해 정의된 안전 영역과 레이아웃 마진에 충실하십시오.

이러한 레이아웃 가이드는 디바이스와 컨텍스트에 기반한 적절한 삽입*insetting*을 보장합니다. 안전 영역은 또한 상태 바, 내비게이션 바, 툴바, 그리고 탭 바 아래에 컨텐츠가 덮어지는 것을 방지합니다. 시스템이 제공하는 표준 뷰는 자동으로 안전 영역 레이아웃 가이드*safe area layout guide*를 채택합니다.

개발자 가이드에서, [UILayoutGuide](https://developer.apple.com/documentation/uikit/uilayoutguide), [layoutMarginsGuide](https://developer.apple.com/documentation/uikit/uiview/1622651-layoutmarginsguide), [readableContentGuide](https://developer.apple.com/documentation/uikit/uiview/1622644-readablecontentguide), [safeAreaLayoutGuide](https://developer.apple.com/documentation/uikit/uiview/2891102-safearealayoutguide)를 참고하십시오.

## 사이즈 클래스*Size Classes*

사이즈 클래스는 그 크기에 기반하여 컨텐츠 영역에 자동으로 할당되는 특징*traits*입니다. 시스템은 두 가지의 사이즈 클래스, *regular*(확장 공간을 나타냄)와 *compact*(제한된 공간을 나타냄)를 정의하여 뷰의 높이와 너비를 묘사합니다.

뷰는 어떠한 사이즈 클래스의 조합이라도 소유할 수 있습니다.

- Regular 너비, Regular 높이
- Compact 너비, Compact 높이
- Regular 너비, Compact 높이
- Compact 너비, Regular 높이

다른 환경 변수처럼, iOS는 컨텐츠 영역의 사이즈 클래스에 기반하여 동적으로 레이아웃을 조절합니다. 예를 들어, 사용자가 세로 방향에서 가로 방향으로 디바이스를 회전시켜, 수직적 사이즈 클래스가 compact 높이에서 regular 높이로 변경될 때, 탭 바는 더 길어질 것입니다.

### 디바이스 사이즈 클래스

다른 사이즈 클래스 조합은 화면 크기에 기반한, 다른 디바이스의 전체 화면 경험에 적용됩니다.

| 디바이스                                    | 세로 방향                  | 가로 방향                  |
| ------------------------------------------- | -------------------------- | -------------------------- |
| iPad                                        | Regular 너비, Regular 높이 | Regular 너비, Regular 높이 |
| iPhone 큰 화면 모델<br />(Plus, XS MAX, XR) | Compact 너비, Regular 높이 | Regular 너비, Compact 높이 |
| 나머지 iPhone                               | Compact 너비, Regular 높이 | Compact 너비, Compact 높이 |

### 사이즈 클래스 멀티태스킹

iPad에서, 사이즈 클래스는 애플리케이션이 [멀티태스킹](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/multitasking/) 구성에서 동작할 때도 적용됩니다.

| 기기          | 모드          | 세로 방향                  | 가로 방향                  |
| ------------- | ------------- | -------------------------- | -------------------------- |
| 12.9인치 iPad | 2/3 스플릿 뷰 | Compact 너비, Regular 높이 | Regular 너비, Regular 높이 |
|               | 1/2 스플릿 뷰 | Compact 너비, Regular 높이 | Regular 너비, Regular 높이 |
|               | 1/3 스플릿 뷰 | Compact 너비, Regular 높이 | Compact 너비, Regular 높이 |
| 나머지 iPad   | 2/3 스플릿 뷰 | Compact 너비, Regular 높이 | Regular 너비, Regular 높이 |
|               | 1/2 스플릿 뷰 | Compact 너비, Regular 높이 | Compact 너비, Regular 높이 |
|               | 1/3 스플릿 뷰 | Compact 너비, Regular 높이 | Compact 너비, Regular 높이 |

## 일반적인 레이아웃 고려사항*General Layout Considerations*

### 주요 컨텐츠가 그 기본 크기에서 명백한 것을 확실하게 하십시오.

사람들은 크기를 변경하는 것을 선택하지 않는 한, 중요한 텍스트를 읽기 위해 수평으로 스크롤하거나, 주요 이미지를 보기 위해 확대해야 하지 않아야 합니다.

### 애플리케이션 도처에서 전반적으로 일관성 있는 모습을 유지하십시오.

일반적으로, 비슷한 기능을 하는 요소는 비슷하게 보여야 합니다.

### 중요성을 투영하기 위해 시각적인 두께*weight*와 균형*balance*를 사용하십시오.

큰 아이템은 눈을 사로잡아 작은 것보다 더 중요해 보입니다. 큰 아이템은 탭하기도 더 쉬워, 주방이나 체육관과 같은 산만한 환경에서 애플리케이션이 사용될 때 특히 중요합니다. 일반적으로, 화면 절반의 윗부분에 주요한 아이템을 배치하고, 왼쪽에서 오른쪽으로 읽어나가는 컨텍스트에서 화면의 왼쪽 가장자리와 가깝게 배치하십시오.

### 스캐닝을 쉽게 하고 구성과 계층과 소통하기 위해 정렬을 사용하십시오.

정렬은 애플리케이션이 산뜻하고 조직된 것처럼 보이게 하여, 사람들이 스크롤하는 동안 집중하는 것을 돕고, 정보를 찾는 것을 더 쉽게 해줍니다. 들어쓰기*indentation*와 정렬은 또한 컨텐츠 그룹이 관련된 방법을 가리킬 수 있습니다.

### 가능하다면, 세로 및 가로 방향을 모두 지원하십시오.

사람들은 다른 방향에서 애플리케이션을 사용하는 것을 선호할 수 있으므로, 그 기대를 충족시킬 때 최상의 효과를 가져옵니다.

### 텍스트 크기 변화에 대비하십시오.

사람들은 설정 앱에서 다른 텍스트 크기를 선택할 때 대부분의 애플리케이션이 적절하게 반응하기를 기대합니다. 몇몇 텍스트 크기 변화에 적응하기 위해, 레이아웃을 조절할 필요가 있을 것입니다. 애플리케이션에서의 텍스트 사용에 대해 더 많은 정보를 알아보기 위해, [타이포그래피](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/)를 참고하십시오.

### 대화형 요소에 충분한 터치 표적을 제공하십시오.

모든 컨트롤에 최소한 44pt x 44pt의 터치 가능한 영역을 유지하려고 노력하십시오.

### 다양한 디바이스에서 애플리케이션을 미리 보이십시오.

애플리케이션을 미리 보고 잘리는 것과 다른 레이아웃 이슈를 확인하기 위해 Xcode에 내장된 Simulator를 사용할 수 있습니다. 애플리케이션이 가로 모드를 지원한다면, 디바이스가 왼쪽으로 회전되었든 오른쪽으로 회전되었든, 레이아웃이 좋게 보이는 것을 확실하게 하십시오. 거꾸로 회전된 세로 모드는 전체 화면 iPhone에서 지원되지 않습니다. 와이드 컬러 형상*wide color imagery*과 같은 일부 기능은 실제 디바이스에서 미리 보는 것이 가장 좋습니다.

### 더 큰 디바이스에서 텍스트를 표시할 때 가독성을 위한 마진을 적용하십시오.

이러한 마진은 편안한 읽기 경험을 보장하기 위해 충분하게 텍스트 줄 수를 짧게 유지합니다.

## 컨텍스트 내 변화에 적응하기*Adapting to Changes in Context*

### 컨텍스트가 변화하는 동안 현재 컨텐츠에 포커스를 유지하십시오.

컨텐츠는 당신의 가장 높은 우선순위입니다. 환경이 변화할 때 포커스를 변경하는 것은 길을 잃고 낙담하게 할 수 있으며, 사람들이 애플리케이션에 대한 통제력을 잃은 것처럼 느끼게 할 것입니다.

### 불필요하게 레이아웃을 변경하지 마십시오.

누군가가 디바이스를 회전시켰을 때, 전체 레이아웃은 변경되지 않아도 됩니다. 예를 들어, 애플리케이션이 세로 모드에서 이미지 격자를 보여준다면, 가로 모드에서 같은 이미지를 목록의 형태로 표시하지 않아도 됩니다. 대신, 격자의 차원을 조절하기만 하면 됩니다. 모든 컨텍스트에서 비슷한 경험을 유지하기 위해 노력하십시오.

### 애플리케이션이 단일 방향에서 실행되는 것이 필수적이라면, 두 가지의 변형 모두를 지원하십시오.

오직 가로 모드에서만 동작하는 애플리케이션은 사용자가 왼쪽 또는 오른쪽으로 디바이스를 회전시키는 것에 관계 없이 사용 가능해야 합니다. 오직 세로 모드에서만 동작하는 애플리케이션은, 위아래가 뒤바뀐 세로 모드를 지원하지 않는 iPhone X를 제외하고, 디바이스를 180도 회전시켰을 때 컨텐츠도 180도 회전되게 해야 합니다. 누군가가 디바이스를 잘못된 방향으로 들고 있을 때 애플리케이션이 자동으로 회전하지 않는다면, 그들은 본능적으로 그것을 회전시켜야 함을 알 것입니다. 그들에게 말할 필요가 없습니다.

### 컨텍스트에 따른 애플리케이션의 회전에 대한 반응을 커스터마이징 하십시오.

예를 들어, 디바이스를 회전시켜 캐릭터를 움직이게 하는 게임은 게임 진행 중에 방향을 전환하지 못해야 할 것입니다. 하지만, 현재 방향에 기반하여 메뉴와 인트로 시퀀스를 표시해야 할 것입니다.

### iPhone 뿐만 아니라, iPad에서도 작동하는 것을 확실하게 하십시오.

사용자는 어느 타입의 iOS 디바이스에서도 애플리케이션이 동작하는 유연함을 갖는 것을 높이 평가합니다. 애플리케이션을 사용하는 대부분의 사람이 iPhone에서 사용하는 것을 기대할지라도, 인터페이스 요소는 iPad에서 가시적이고 기능적이게 되어야 합니다. 애플리케이션의 특정 기능이 3D 터치와 같은 iPhone에 특화된 하드웨어를 필요로 한다면, iPad에서는 이러한 기능들을 숨기거나 비활성화하고 다른 기능을 사용하게 하는 것을 고려하십시오.

### 존재하는 아트워크를 재사용할 때 종횡비*aspect ratio*의 차이를 염두에 두십시오.

다른 화면 크기는 다른 종횡비를 가질 것이며, 이것은 아트워크가 잘리고, 우편함처럼 되고*letterboxed*, 또는 우체통처럼 되는*pillarboxed* 결과를 불러 일으킵니다. 중요한 시각적 컨텐츠는 모든 디스플레이 크기에서 계속 볼 수 있는 것을 확실하게 하십시오.

> letterboxed : 4.7인치용 기존 아트워크의 너비를 같게 하여 위아래가 비어 보이는 것처럼.
>
> pillarboxed : 5.8인치용(iPhone X용) 기존 아트워크의 높이를 같게 하여 양 옆이 비어 보이는 것처럼.

## 전체 화면 경험 디자인하기*Designing a Full-Screen Experience*

### 화면을 채우기 위해 시각적 요소를 확장하십시오.

배경을 화면의 가장자리로 확장하고, 테이블과 컬렉션과 같은 수직으로 스크롤 가능한 레이아웃은 맨 아래까지 계속 이어지는 것을 확실하게 하십시오.

### 화면의 바로 맨 아래와 모서리 부분에 대화형 컨트롤을 명시적으로 배치하지 마십시오.

사람들은 홈 화면과 앱 전환기 같은 기능에 접근하기 위해 디스플레이의 하단 가장자리에서 스와이프 제스처를 사용하고, 이러한 제스처들은 이 영역에서 당신이 구현한 사용자 정의 제스처를 취소시킬 것입니다. 화면의 모서리 부분은 사람들이 편안하게 도달하기 힘든 영역일 수 있습니다.

### 잘림을 방지하기 위해 필수 컨텐츠에 인셋*inset*을 부여하십시오.

일반적으로, 컨텐츠는 중앙에 위치하고 좌우 대칭적으로 인셋을 가져 어느 방향에서나 좋게 보이고, 둥근 모서리에 의해 잘리지 않고, 센서 하우징*sensor housing*에 의해 숨겨지지 않고, 홈 화면에 접근하기 위한 표시기에 의해 잘 보이지 않게 되지 않아야 합니다. 최상의 결과를 위해, 시스템이 제공하는 표준 인터페이스 요소를 사용하고, 인터페이스 구축을 위해 [오토레이아웃](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#auto-layout)을 사용하며, UIKit에 의해 정의된 [레이아웃 가이드와 안전 영역](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#layout-guides-and-safe-area)을 고수하십시오. 디바이스가 가로 방향에 있을 때, 게임과 같은 몇몇 애플리케이션은 컨텐츠를 위한 더 많은 공간을 허용하기 위해 안전 영역 아래로 확장하는 화면의 아래 부분에 탭 가능한 컨트롤을 배치하는 것이 적절할 수 있습니다. 화면의 상단과 하단에 컨트롤을 배치할 때 일치하는 인셋을 사용하고, 홈 지시기 주변에 충분하나 공간을 남겨 두어 사람들이 컨트롤과 상호 작용하려 할 때 실수로 그것을 목표로 삼지 않게 하십시오.

### 전체 너비를 갖는 버튼에 인셋을 부여하십시오.

화면의 가장자리에 확장하는 버튼은 버튼처럼 보이지 않을 수 있습니다. 전체 너비를 갖는 버튼의 양 옆에 있는 표준 UIKit 마진을 준수하십시오. 화면 아래에 나타나는 전체 너비의 버튼은 둥근 모서리가 있고 안전 영역의 하단에 정렬되어 있을 때 가장 좋아 보입니다. 이것은 또한 홈 지시기와 충돌하지 않는다는 것을 보장합니다.

### 주요한 디스플레이 특징에 마스크나 특별한 주의를 기울이지 마십시오.

검은 바를 화면의 상단과 하단에 배치하여 디바이스의 둥근 모서리, 센서 하우징, 또는 홈 화면에 접근하기 위한 지시기를 가리는 것을 시도하지 마십시오. 또한 이러한 영역에 특별한 주의를 기울이기 위해 브래킷*bracket*s, 베젤, 도형, 또는 지시적인 텍스트와 같은 시각적 장식을 사용하지 마십시오.

###  상태 바의 높이를 염두에 두십시오.

상태 바는 구식 IPhone보다 전체 화면 iPhone에서 더 깁니다. 애플리케이션이 상태 바 아래에 컨텐츠를 배치하기 위해 고정된 상태 바 높이를 가정한다면, 사용자의 디바이스에 기반하여 컨텐츠의 배치를 동적으로 갱신해야 합니다. 전체 화면 iPhone에 있는 상태 바는 목소리 녹음과 위치 추적과 같은 백그라운드 작업이 활성화되어 있을 때 높이를 변경하지 않음을 기억하십시오.

### 애플리케이션이 현재 상태 바를 가리고 있다면, 전체 화면 iPhone을 위해 그 결정을 다시 고려하십시오.

전체 화면 iPhone은 구식 iPhone보다 컨텐츠를 위한 더 많은 수직 공간을 가지고 있으며, 상태 바는 애플리케이션이 완전하게 활용하지 못할 화면의 어떤 영역을 차지합니다. 상태 바는 또한 사람들이 유용하게 찾는 정보를 표시합니다. 부가 가치에 대한 대가로만 그것은 숨겨져야 합니다.

### 드물게 홈 화면에 접근하기 위한 표시기를 자동으로 숨기는 것을 허용하십시오.

자동 숨김이 활성화되어 있을 때, 지시기는 사용자가 몇 초 동안 화면을 터치하지 않는다면 사라져 갑니다. 사용자가 화면을 다시 터치했을 때 다시 나타납니다. 이 행동은 비디오 시청 또는 사진 슬라이드쇼와 같은 수동적인 시청 경험에 대해서만 활성화되어야 합니다.

## 추가적인 레이아웃 고려사항*Additional Layout Considerations*

### 당신의 웹사이트가 최첨단*edge-to-edge* 디스플레이에서 좋아 보인느 것을 확실하게 하십시오.

[webkit.org](https://webkit.org/)에 있는 [iPhone X를 위한 웹사이트 디자인하기](https://webkit.org/blog/7929/designing-websites-for-iphone-x/)를 참고하십시오.