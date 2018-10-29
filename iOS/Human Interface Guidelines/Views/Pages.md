[Human Interface Guidelines - Views - Pages](https://developer.apple.com/design/human-interface-guidelines/ios/views/pages/)

# 페이지*Pages*

페이지 뷰 컨트롤러는 문서, 책, 노트패드 또는 캘린더와 같은 컨텐츠의 페이지 사이에서 선형 탐색을 구현하는 방법을 제공합니다. 페이지 뷰 컨트롤러는 탐색 동안 페이지 사이 전환을 관리하는, 스크롤 또는 책장 넘김*page-curl*의 두 가지 스타일 중 하나를 사용합니다. 스크롤 전환은 독특한 모습을 하지 않으며, 페이지는 한 페이지에서 다른 페이지로 유려하게 스크롤됩니다. 책장 넘김 전환은 당신이 화면을 가로질러 스와이프할 때, 물리적 책에서 페이지를 넘기는 것처럼 페이지가 넘어가는 것을 유발합니다.

### 적절하다면, 비선형적으로 탐색할 수 있는 방법을 구현하십시오.

페이지 뷰 컨트롤러를 사용할 때, 페이지는 순차적으로 흐르며 인접하지 않은 페이지 사이를 뛰어넘는 방법은 없습니다. 사람들이 애플리케이션에서 시퀀스 밖에 있는 페이지에 접근할 필요가 있다면, 이 기능을 제공하는 커스텀 컨트롤을 구현하십시오.

개발자 가이드에서, [UIPageVIewController](https://developer.apple.com/documentation/uikit/uipageviewcontroller)를 참고하십시오.