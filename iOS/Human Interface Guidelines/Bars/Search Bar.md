[Human Interface Guidelines - Bars - Search Bars](https://developer.apple.com/design/human-interface-guidelines/ios/bars/search-bars/)

# 검색 바*Search Bars*

검색 바는 필드에 텍스트를 타이핑하여 값의 거대한 컬렉션을 통해 검색하는 것을 가능하게 합니다. 감색 바는 단독으로 표시되거나, 내비게이션 바 또는 컨텐츠 뷰 내에서 표시될 수 있습니다. 내비게이션 바에 표시될 때, 검색 바는 항상 접근 가능하도록 내비게이션 바에 고정될 수 있으며, 사용자가 그것을 드러내기 위해 아래로 스와이프할 때까지 접어질*collapsed* 수 있습니다.

### 검색을 구현하기 위해 텍스트 필드 대신 검색 바를 사용하십시오.

텍스트 필드는 사람들이 기대하는 표준 검색 바의 모습을 하고 있지 않습니다.

### 클리어*Clear* 버튼을 활성화 하십시오.

대부분의 검색 바는 필드의 컨텐츠를 지우는 클리어 버튼을 포함합니다.

### 적절할 때에 취소*Cancel* 버튼을 활성화 하십시오.

대부분의 헌신적인 검색 바는 즉시 검색을 종료하는 취소 버튼을 포함합니다.

### 필요하다면, 검색 바에 힌트와 컨텍스트를 제공하십시오.

검색 바의 필드는 검색되기 시작하는 컨텍스를 암시하는, "옷, 신발 및 악세사리 검색" 또는 간단하게 "검색"과 같은 플레이스홀더 텍스트를 포함할 수 있습니다. 적절한 구두점을 포함하는 간결한, 한 줄의 프롬프트는 사람들이 회사명이나 주식 기호로 진입할 수 있다는 것을 알려줍니다.

### 검색 바 아래에 유용한 바로 가기 및 다른 컨텐츠를 제공하는 것을 고려하십시오.

사람들이 컨텐츠에 더 빠르게 접근하는 것을 돕기 위해 검색 바 아래 영역을 사용하십시오. 예를 들어, 사파리 앱은 검색 필드를 탭하자마자 북마크를 보여줍니다. 어떠한 검색 용어를 입력하지 않아도 하나를 선택하여 그것에 곧바로 나아갑니다. 주식 앱은 검색 필드에 입력한 결과 목록을 보여줍니다. 더 많은 문자를 입력하지 않고 언제든지 하나를 탭하십시오.

개발자 가이드에서, [UISearchController](https://developer.apple.com/documentation/uikit/uisearchcontroller)와 [UISearchBar](https://developer.apple.com/documentation/uikit/uisearchbar)를 참고하십시오.

## 스코프 바*Scope Bars*

스코프 바는 검색 바에 추가되어 검색 스코프를 정의하는 것을 가능하게 합니다.

### 스코프 바를 포함하여 향상된 검색 결과를 지지하십시오.

스코프 바는 검색하려는 명백히 정의된 범주가 있을 때 유용할 수 있습니다. 그러나, 검색 결과를 향상시켜 스코프가 불필요하게 하는 것이 최상입니다.

개발자 가이드에서, [UISearchBar](https://developer.apple.com/documentation/uikit/uisearchbar)를 참고하십시오.

