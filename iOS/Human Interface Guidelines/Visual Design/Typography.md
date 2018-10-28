[Human Interface Guidelines - Visual Design - Typography](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/)

# 타이포그래피*Typography*

> 관련 테이블은 원문에서 확인하기

San Francisco(SF)는 iOS의 시스템 서체입니다. 이 서체의 글꼴들은 텍스트에게 일치하지 않는 읽기 쉬움*legibility*, 뚜렷함 및 일관성을 주기 위해 최적화 되었습니다. [여기](https://developer.apple.com/fonts/)에서 San Francisco 글꼴 패밀리를 다운로드 하십시오.

### 중요한 정보를 강조하십시오.

애플리케이션에서 가장 중요한 정보를 강조하기 위해 글꼴의 두께, 크기 및 색상을 사용하십시오.

### 가능하다면, 단일 서체를 사용하십시오.

여러 가지 다른 서체를 섞는 것은 애플리케이션이 분열되어 있고 되는 대로 만들어진 것이라고 보이게 합니다. 하나의 서체를 사용하고 단지 몇 개의 글꼴의 변형과 크기를 사용하는 것을 고려하십시오.

### 가능할 때마다 내장된 텍스트 스타일을 사용하십시오.

내장된 텍스트 스타일은 시각적으로 분리된 방식들로, 최적화된 읽기 쉬움을 유지하면서 컨텐츠를 표현할 수 있게 합니다. 이러한 스타일들은 시스템 글꼴에 기반하며, 모든 글꼴 크기에 대해 자동으로 추적하고 시작을 조절하는 동적 타입*Dynamic Type*과 같은 주요한 타이포그래픽적 기능의 이점을 사용 가능하게 합니다. iOS는 다음의 텍스트 스타일을 포함합니다.

- 큰 제목*Large Title*
- 제목 1*Title 1*
- 제목 2*Title 2*
- 제목 3*Title 3*
- 큰 표제*Headline*
- 본문*Body*
- 호출*Callout*
- 부제*Subhead*
- 각주*Footnote*
- 설명 1*Caption 1*
- 설명 2*Caption 2*

개발자 가이드에서, [UIFontTextStyle](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/)을 참고하십시오.

### 커스텀 글꼴이 읽기 쉬운 것을 확실하게 하십시오.

커스텀 서체는 iOS에서 지원되지만, 종종 읽기 힘듭니다. 브랜딩 목적이나 몰입감 있는 게임의 경험을 만드는 것과 같은, 애플리케이션이 커스텀 폰트에 대한 필요를 강제하지 않는 한, 시스템 글꼴을 고수하는 것이 보통 최상의 결과를 가져옵니다. 커스텀 글꼴을 사용한다면, 작은 크기에서도 읽기 쉬운 것을 확실하게 하십시오.

### 커스텀 글꼴에 손쉬운 사용*accessibility* 기능을 구현하십시오.

시스템 글꼴은 볼드 텍스트 및 더 큰 타입과 같은 손쉬운 사용 기능에 자동으로 반응합니다. 커스텀 글꼴을 사용하는 애플리케이션은 손쉬운 사용 기능이 활성화되어 있는지, 그것들이 변할 때의 알림을 등록해 두었는지 확인하여 같은 행동을 구현해야 합니다. [손쉬운 사용](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/)을 참고하십시오.

## 동적 타입 크기*Dynamic Type Sizes*

San Francisco 서체는 작고 큰 크기 모두에서 높은 가독성을 얻기 위해 디자인 되었습니다. 동적 타입은 독자들이 선호하는 텍스트 크기를 선택하게 함으로서 추가적인 유연함을 제공합니다. [리소스](https://developer.apple.com/design/resources/#ios-apps)에서 동적 타입 크기 표를 다운로드 하십시오.

### 텍스트 크기의 변화에 응답할 때 컨텐츠에 우선 순위를 매기십시오.

모든 컨텐츠가 동등하게 중요한 것은 아닙니다. 누군가가 더 큰 크기를 선택할 때, 그들은 관심 있는 컨텐츠를 읽기 더 쉽게 만들기를 원합니다. 화면 상에 있는 모든 단어가 크게 되는 것을 항상 원하지는 않습니다.

xSmall / Small / Medium / Large (기본값) / xLarge / xxLarge / xxxLarge

## 더 큰 접근성 타입 크기*Larger Accessibility Type Sizes*

표준 동적 타입 크기에 더하여, 시스템은 접근성을 필요로 하는 사용자를 위한 몇 가지 훨씬 더 큰 타입 크기를 제공합니다.

AX1 / AX2 / AX3 / AX4 / AX5

## 글꼴 사용 및 트래킹*Font Usage and Tracking*

### 인터페이스 목업*mockups*에서 올바른 글꼴 변형을 사용하십시오.

버튼과 레이블 같은 표준 컨트롤의 텍스트에 San Francisco를 사용할 때, iOS는 포인트 크기와 사용자의 손쉬운 사용 설정에 기반하여 가장 적절한 변형을 적용합니다. 인터페이스 목업에서, 19포인트 이하의 텍스트에 대해서 SF Pro Text를, 20포인트 이상의 텍스트에 대해서 SF Pro Display를 사용하고, 자간을 적절하게 조절하십시오.