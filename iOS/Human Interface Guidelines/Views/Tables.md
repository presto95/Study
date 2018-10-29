[Human Interface Guidelines - Views - Tables](https://developer.apple.com/design/human-interface-guidelines/ios/views/tables/)

# 테이블*Tables*

테이블은 섹션이나 그룹으로 나누어질 수 있는, 스크롤되는 단일 열의 행 목록으로 데이터를 나타냅니다. 테이블을 사용하여 크거나 작은 양의 정보를 목록의 형태로 깨끗하고 효율적으로 표시하십시오. 일반적으로, 테이블은 텍스트 기반 컨텐츠에 이상적이며, 종종 반대쪽에서 보이는 컨텐츠와 관련된, 스플릿 뷰의 한 쪽에서 탐색의 수단으로 나타납니다. [스플릿 뷰](https://developer.apple.com/design/human-interface-guidelines/ios/views/split-views/)를 참고하십시오.

iOS는 두 가지 스타일의 테이블, 일반*plain* 또는 그룹*grouped*을 구현합니다.

- **Plain.** 행은 레이블이 붙은 섹션 안으로 분리될 수 있으며, 선택적인 색인이 테이블의 우측 가장자리를 따라 수직으로 나타날 수 있습니다. 헤더는 섹션의 첫 번째 아이템 이전에 나타날 수 있으며, 푸터는 마지막 아이템 다음에 나타날 수 있습니다.
- **Grouped.** 행은 그룹으로 표시되는데, 앞에는 헤더가 있고 뒤에는 푸터가 있을 수 있습니다. 이 스타일의 테이블은 항상 적어도 하나의 그룹을 포함하며 각각의 그룹은 항상 적어도 하나의 행을 포함합니다. 그룹 테이블은 색인을 포함하지 않습니다.

### 테이블 너비에 대해 생각하십시오.

얇은 테이블은 잘림과 줄바꿈을 유발할 수 있어, 거리를 두고 빠르게 읽거나 스캔하는 것을 어렵게 합니다. 넓은 테이블 또한 읽고 스캔하는 것이 어려울 수 있으며, 컨텐츠로부터 공간을 빼앗을 수 있습니다.

### 테이블 컨텐츠를 빠르게 보여주는 것을 시작하십시오.

무언가가 보여지기 전 큰 테이블 컨텐츠를 로드하는 것을 기다리지 마십시오. 화면 상의 행을 텍스트 데이터로 즉시 채워 넣고 이미지와 같은 더 복잡한 데이터는 사용 가능할 때 보여주십시오. 이 처리 방식은 즉시 유용한 정보를 주며 애플리케이션에 대한 인지된 반응을 증가시킵니다. 몇몇 경우에, 진부하고 오래된 데이터를 보여주는 것이 신선하고 새로운 데이터가 도착할 때까지 말이 될 수 있습니다.

### 컨텐츠가 로드될 때 진행 상황을 전달하십시오.

테이블의 데이터가 로드되기에 시간이 걸린다면, 애플리케이션이 여전히 실행중임을 다시 보장하기 위해 프로그레스 바 또는 회전하는 액티비티 인디케이터를 표시하십시오.

### 컨텐츠를 최신 상태로 유지하십시오.

더 새로운 데이터를 반영하기 위해 테이블의 컨텐츠를 주기적으로 갱신하는 것을 고려하십시오. 단지 스크롤 위치를 바꾸는 것이 아닙니다. 대신에, 테이블의 시작이나 끝에 컨텐츠를 더하고, 그것들이 준비될 때 그것으로 스크롤하게 하십시오. 몇몇 애플리케이션은 새로운 데이터가 추가될 때 표시기를 표시하며, 그것으로 바로 뛰어들어가는 컨트롤을 제공합니다. 리프레시 컨트롤을 포함하는 것도 좋은 방법이며, 언제든지 수동으로 업데이트를 수행할 수 있게 합니다. [리프레스 컨텐츠 컨트롤](https://developer.apple.com/design/human-interface-guidelines/ios/controls/refresh-content-controls/)을 참고하십시오.

### 색인과 우측 정렬된 요소를 포함하는 테이블 행을 결합하지 마십시오.

색인은 큰 스와이프 제스처를 수행하기 위해 컨트롤됩니다. 공개 지시기*disclosure indicators*와 같은 다른 대화형 요소가 근처에 있다면, 제스처가 발생할 때 사용자의 의도를 인식하기 어려울 수 있고 잘못된 요소가 활성화될 수 있습니다.

개발자 가이드에서, [UITableView](https://developer.apple.com/documentation/uikit/uitableview)를 참고하십시오.

## 테이블 행*Table Rows*

- **Basic(기본값).** 행의 좌측 가장자리에 있는 선택적인 이미지 다음에 좌측 정렬된 타이틀이 따라옵니다. 부가 정보를 필요로 하지 않는 아이템을 표시하기 위한 좋은 선택지입니다. 개발자 가이드에서, [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)의 [UITableViewCellStyleDefault](https://developer.apple.com/documentation/uikit/uitableviewcellstyle/uitableviewcellstyledefault) 상수를 참고하십시오.
- **Subtitle.** 한 줄에 좌측 정렬된 타이틀과 다음 줄에 좌측 정렬된 부제가 있습니다. 이 스타일은 시각적으로 비슷한 행을 갖는 테이블에서 잘 동작합니다. 추가적인 부제는 행을 다른 것들로부터 구별하는 것을 돕습니다. 개발자 가이드에서, [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)의 [UITableViewCellStyleSubtitle](https://developer.apple.com/documentation/uikit/uitableviewcellstyle/uitableviewcellstylesubtitle) 상수를 참고하십시오.
- **Right Detail (Value 1).** 같은 줄에 좌측 정렬된 타이틀과 우측 정렬된 부제가 있습니다. 개발자 가이드에서, [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)의 [UITableViewCellStyleValue1](https://developer.apple.com/documentation/uikit/uitableviewcellstyle/uitableviewcellstylevalue1) 상수를 참고하십시오.
- **Left Detail (Value 2).** 같은 줄에 좌측 정렬된 부제 다음에 우측 정렬된 타이틀이 있습니다. 개발자 가이드에서, [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)의 [UITableViewCellStyleValue2](https://developer.apple.com/documentation/uikit/uitableviewcellstyle/uitableviewcellstylevalue2) 상수를 참고하십시오.

모든 표준 테이블 셀 스타일은 체크 표시 또는 공개 지시기disclosure indicator와 같은 그래픽적 요소를 허용합니다. 물론, 이러한 요소를 더하는 것은 타이틀과 부제가 사용 가능한 공간을 줄입니다.

### 잘리는 것을 피하기 위해 텍스트를 간결하게 유지하십시오.

잘린 단어 및 어구는 스캔 및 판독하기 어렵습니다. 텍스트 잘림은 모든 테이블 셀 스타일에서 자동으로 일어나지만, 당신이 사용하는 셀 스타일이 무엇인지, 어디서 잘림이 일어나는지에 따라 크고 작은 문제를 나타낼 수 있습니다.

### 삭제*Delete* 버튼에 커스텀 타이틀을 사용하는 것을 고려하십시오.

행이 삭제를 지원하고 그것이 뚜렷함을 제공하는 것을 돕는다면, 시스템이 제공하는 삭제 타이틀을 커스텀 타이틀로 교체하십시오.

### 선택되었을 때 피드백을 제공하십시오.

사람들은 컨텐츠가 탭 되었을 때 간결하게 강조하는 것을 기대합니다. 그리고 나서, 체크마크가 나타나는 것과 같은, 선택이 이루어졌음을 가리키는, 새로운 뷰가 나타나거나 무언가가 변화하는 것을 기대합니다.

### 비표준 테이블 행에 커스텀 테이블 셀 스타일을 디자인 하십시오.

표준 스타일은 다양한 일반적인 시나리오에서 사용하기 좋지만, 몇몇 컨텐츠나 전체적인 애플리케이션 디자인은 많이 커스터마이징된 테이블의 모습을 유발할 수 있습니다. 당신 자신만의 셀을 만드는 방법을 배우기 위해, [iOS 테이블 뷰 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/AboutTableViewsiPhone/AboutTableViewsiPhone.html)의 [셀 커스터마이징하기](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW18)를 참고하십시오.

개발자 가이드에서, [UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)을 참고하십시오.