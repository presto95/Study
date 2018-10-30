[Human Interface Guidelines - Extensions - Document Providers](https://developer.apple.com/design/human-interface-guidelines/ios/extensions/document-providers/)

# 문서 제공자*Document Providers*

문서 제공자 확장은 애플리케이션의 문서를 시스템이 있는 다른 애플리케이션 내에서 가져오거나, 내보내거나, 열거나, 이동하기 위핸 커스텀 인터페이스를 구현합니다. 문서 제공자 확장이 로드될 때, 그 인터페이스는 내비게이션 바를 포함하는 모달 뷰 내에 표시됩니다.

### 사용자가 파일을 열거나 가져올 때, 오직 컨텍스트에 특화된 문서와 정보만을 보여주십시오.

누군가가 당신의 확장을 문서를 열거나 가져오기 위해 사용할 때, 현재 컨텍스트에 적절한 문서만을 표시하십시오. 예를 들어, PDF 편집 애플리케이션이 당신의 확장을 로드한다면, 열거나 가져올 수 있는 가능한 문서로서 PDF 파일의 목록만을 표시하십시오. 수정 날짜, 크기, 문서가 로컬에 있는지 원격에 있는지와 같은, 또한 유용할 다른 정보를 나열하는 것을 보장하십시오.

### 사람들이 문서를 내보내고 이동할 때 목적지를 선택하게 하십시오.

애플리케이션이 하나의 디렉토리에 문서를 저장하지 않는 한, 사용자가 디렉토리 계층에서 특정한 목적지로 탐색할 수 있게 하십시오. 새로운 서브 디렉토리를 추가하는 방법을 제공하는 것을 고려하십시오.

### 커스텀 내비게이션 바를 제공하지 마십시오.

당신의 확장은 이미 내비게이션 바를 포함하는 모달 뷰 내에 로드됩니다. 부가적인 내비게이션 바를 제공하는 것은 혼란스럽고 컨텐츠로부터 멀어지게 합니다.

개발자 가이드에서, [애플리케이션 확장 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/index.html)의 [문서 제공자](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/FileProvider.html)를 참고하십시오.