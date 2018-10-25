[Human Interface Guidelines - App Architecture - Modality](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/modality/)

# 모달리티*Modality*

모달리티는 사람들이 작업을 완료하거나 메세지 또는 뷰를 사라지게 할 때까지 다른 것을 하는 것을 못하게 함으로서 포커스를 만듭니다. 액션 시트*action sheets*, 얼러트*alerts*, 그리고 액티비티 뷰*activity views*는 모달 경험을 제공합니다. 모달 뷰가 화면 상에 나타낼 때, 사용자는 버튼을 누르거나 그렇지 않으면 모달 경험을 종료함으로써 선택을 해야만 합니다. 몇몇 애플리케이션은 캘린더 앱의 이벤트를 편집하는 것 또는 사파리 앱에서 북마크를 선택하는 것과 같은 모달 뷰를 구현합니다. 보달 뷰는 전체 화면, 팝오버 처럼 전체 부모 뷰, 또는 화면의 일부분을 차지할 수 있습니다. 모달 뷰는 일반적으로 완료 버튼과 뷰를 종료하기 위한 취소 버튼을 포함합니다.

### **모달리티의 사용을 최소화 하십시오.** 

일반적으로, 사람들은 비선형적인 방법으로 애플리케이션과 상호작용하는 것을 선호합니다. 누군가의 집중을 얻는 것이 중요할 때, 애플리케이션 사용을 계속하기 위해 작업이 완료되거나 버려져야 할 때, 또는 중요한 데이터를 저장할 때만 모달 컨텍스트를 만드는 것을 고려하십시오.

### **모달 작업을 종료하기 위한 명백하고 안전한 방법을 제공하십시오.** 

사람들이 모달 뷰를 사라지게 했을 때 그들의 액션에 대한 결과를 항상 알도록 하는 것을 확실하게 하십시오.

### **모달 작업을 간단하고, 짧고, 좁은 것에 초점을 맞추게 하십시오.** 

애플리케이션 안에 애플리케이션을 만들지 마십시오. 모달 작업이 너무 복잡하다면, 사람들은 모달 컨텍스트에 진입했을 때 그들이 보류한 작업에 대한 시야를 잃을 수 있습니다. 특히 사용자가 길을 잃고 단계를 추적하는 방법을 잊어버릴 수 있기 때문에 뷰 계층을 포함하는 모달 작업을 만드는 것에 주의하십시오. 모달 작업이 서브뷰를 포함해야 한다면, 계층과 완료를 위한 명백한 경로를 통해 단일 경로를 제공하십시오. 작업 완료 이외의 작업에는 완료 버튼을 사용하지 마십시오.

### **적절한 경우, 작업을 식별하는 타이틀을 표시하십시오.** 

더 충분하게 작업을 묘사하거나 가이드를 제공하는 뷰의 다른 부분에 텍스트를 제공할 수도 있습니다.

### **필수적인, 이상적으로는 액션이 가능한 정보를 제공하기 위한 얼러트를 표시하십시오.** 

얼러트는 경험을 가로채고 사라지게 하기 위한 탭을 필요로 하는데, 그래서 사람들이 이 개입에 정당한 이유가 있다고 느끼게 하는 것은 중요합니다. 더 배우기 위해, [얼러트](https://developer.apple.com/design/human-interface-guidelines/ios/views/alerts/)를 참고하십시오.

### **알림 환경 설정을 존중하십시오.** 

설정 앱에서, 사람들은 애플리케이션에서 알림을 받기 원하는 방법을 특정합니다. 이러한 환경 설정에 따라 애플리케이션에서의 알림을 완전히 끄는 것을 유혹받지는 않습니다.

### **모달 뷰를 팝오버 위에 표시하지 마십시오.** 

얼러트의 가능한 예외와 함께, 어떠한 것도 팝오버 위에 나타나서는 안됩니다. 팝오버에서 일어나는 액션 후에 모달 뷰를 표시할 필요가 있는 드문 경우에, 모달 뷰를 표시하기 전 팝오버를 닫으십시오.

### **애플리케이션과 모달 뷰의 생김새를 동등하게 하십시오.** 

예를 들어, 모달 뷰는 내비게이션 바를 포함할 수 있습니다. 이 경우 애플리케이션에서 사용하는 내비게이션 바와 같은 생김새를 사용하십시오.

### **적절한 모달 뷰 스타일을 선택하십시오.** 

아래의 스타일 중 어떠한 것을 사용할 수 있습니다.

- **전체 화면*Full Screen***. 전체 화면을 덮습니다. 모달 뷰의 컨텍스트 내에서 완료될 필요가 있는 잠재적으로 복잡한 작업을 위해 사용하십시오.
- **페이지 시트*Page Sheet***. 가로 방향의 넓은 디바이스에서 아래에 있는 컨텐츠를 부분적으로 덮습니다. 덮혀지지 않은 모든 부분은 상호 작용을 막기 위해 희미해집니다. 작은 디바이스와 가로 방향에서는 전체 화면을 덮습니다. 모달 뷰의 컨텍스트 내에서 완료될 필요가 있는 잠재적으로 복잡한 작업을 위해 사용하십시오.
- **폼 시트*Form Sheet***. 화면의 중앙에 나타나지만, 키보드가 보여진다면 다시 위치할 것입니다. 덮혀지지 않은 모든 부분은 상호 작용을 막기 위해 희미해집니다. 작은 디바이스에서는 전체 화면을 덮을 것입니다. 정보를 수집하기 위해 사용하십시오.
- **현재 컨텍스트*Current Context***. 그것의 부모 뷰와 같은 크기로 보여집니다. 스플릿 뷰의 구획, 팝오버, 또는 풀스크린이 아닌 다른 뷰 내에서 모달 컨텐츠를 표시하기 위해 사용하십시오.

### **모달 뷰를 드러내기 위한 적절한 전환 스타일을 선택하십시오.** 

애플리케이션과 조화를 이루고 일시적인 컨텍스트 변화에 대한 인식을 강화하기 위한 전환 스타일을 사용하십시오. 기본 전환은 모달 뷰를 화면의 아리에서 위로 수직적으로 슬라이드하며 사라질 때는 아래로 내려갑니다. 플립 스타일 전환은 모달 뷰를 드러내기 위해 수평적으로 뷰를 플합니다. 시각적으로 모달 뷰는 현재 뷰의 뒤쪽인 것처럼 보입니다. 애플리케이션 도처에서 일관된 모달 전환 스타일을 사용하십시오.

모달 뷰 개발자 가이드를 위해, [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)와 [UIPresentationController](https://developer.apple.com/documentation/uikit/uipresentationcontroller)를 참고하십시오.