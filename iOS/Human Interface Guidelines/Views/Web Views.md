[Human Interface Guidelines - Views - Web Views](https://developer.apple.com/design/human-interface-guidelines/ios/views/web-views/)

# 웹 뷰*Web Views*

웹 뷰는 삽입된 HTML 및 웹사이트와 같은 풍부한 웹 컨텐츠를 로드하고 애플리케이션 내에 직접적으로 표시합니다. 예를 들어, 메일 앱은 웹 뷰를 사용하여 메세지 내에 있는 HTML 컨텐츠를 표시합니다.

### 적절할 때 앞뒤 탐색을 활성화 하십시오.

웹 뷰는 앞으로 및 뒤로 탐색을 지원하지만, 이 행동은 기본적으로 비활성화되어 있습니다. 사람들이 많은 페이지를 방문할 수 있게 웹 뷰를 사용할 것이라면, 앞으로 및 뒤로 탐색을 활성화하고, 이렇나 기능을 개시하기 위한 상응하는 컨트롤을 제공하십시오.

### 웹 브라우저를 만들기 위해 웹 뷰를 사용하지 마십시오.

애플리케이션의 컨텍스트를 떠나지 않고 간단하게 웹사이트에 접근할 수 있게 하기 위해 웹 뷰를 사용하는 것은 좋지만, 사파리 앱은 iOS에서 웹을 둘러보는 주요한 방법입니다. 애플리케이션에서 사파리 앱의 기능을 복제하는 시도를 하는 것은 불필요하고 장려되지 않습니다.

개발자 가이드에서, [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)를 참고하십시오.