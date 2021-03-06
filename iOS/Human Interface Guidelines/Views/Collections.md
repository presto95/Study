[Human Interface Guidelines - Views - Collections](https://developer.apple.com/design/human-interface-guidelines/ios/views/collections/)

# 컬렉션*Collections*

컬렉션은 사진 집합과 같은 정렬된 컨텐츠 집합을 관리하며, 그것을 커스터마이징 가능하고 높은 시각적 레이아웃으로 나타냅니다. 컬렉션은 엄격하게 선형 포맷을 강제하지 않기 때문에, 특히 크기에서 다양한 아이템을 표시할 때 적합합니다. 일반적으로, 컬렉션은 이미지 기반 컨텐츠를 보여줄 때 이상적입니다. 배경 및 다른 장식적인 뷰는 아이템의 부분집합을 시각적으로 구분하기 위해 선택적으로 구현될 수 있습니다.

컬렉션은 상호 작용성 및 애니메이션을 모두 지원합니다. 기본적으로, 선택하기 위해 탭할 수 있고, 편집하기 위해 터치한 후 유지할 수 있으며, 스크롤하기 위해 스와이프할 수 있습니다. 애플리케이션이 그것을 필요로 한다면, 더 많은 제스처가 커스텀 액션을 수행하기 위해 추가될 수 있습니다. 컬렉션 내에서, 애니메이션은 아이템이 삽입, 삭제 및 재정렬될 때 활성화될 수 있으며, 커스텀 애니메이션 또한 지원됩니다.

### 표준 행 또는 격자 레이아웃이 충분할 때, 급진적인 새로운 디자인을 만들지 마십시오.

컬렉션은 사용자 경험을 향상시켜야 하지, 주의의 중심이 되어서는 안됩니다. 아이템을 선택하는 것을 쉽게 하십시오. 컬렉션에서 아이템을 탭하는 것이 어렵다면, 사람들은 그들이 원하는 컨텐츠에 도달하기 전에 낙담하고 흥미를 잃을 것입니다. 컨텐츠 주위에 적당한 패딩을 사용하여 깨끗한 레이아웃을 유지하고 컨텐츠가 겹쳐지는 것을 방지하십시오.

### 텍스트에 대해서는 컬렉션 대신 테이블을 사용하는 것을 고려하십시오.

스크롤 가능한 목록으로 표시될 때, 텍스트로 된 정보를 보여주고 요약하는 것이 일반적으로 더 간단하고 더 효율적입니다.

### 동적 레이아웃 변경을 할 때 주의를 사용하십시오.

컬렉션의 레이아웃은 어느 때나 변경될 수 있습니다. 사람들이 그것을 보고 상호 작용하고 있는 동안 동적으로 레이아웃을 변경한다면, 말이 되게 바꾸고 추적하기 쉬운 것을 보장하십시오. 이유가 없는 레이아웃 변경은 애플리케이션을 예측하기 힘들고 사용하기 어려운 것처럼 보이게 할 수 있습니다. 컨텍스트가 레이아웃 변경에 의해 사라졌다면, 사람들은 더이상 통제할 수 없다고 느낄 것입니다.

개발자 가이드에서, [UICollectionView](https://developer.apple.com/documentation/uikit/uicollectionview)를 참고하십시오.