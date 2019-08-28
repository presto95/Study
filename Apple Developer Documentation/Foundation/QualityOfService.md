# QualityOfService

**열거형** | 시스템에게 작업의 본질과 중요성을 알리기 위해 사용됨. 더 높은 서비스 퀄리티 클래스를 사용하는 작업은 리소스 다툼이 일어날 때마다 더 낮은 서비스 퀄리티 서비스를 사용하는 작업보다 더 많은 리소스를 받는다.

## userInteractive

상호 작용하는 UI를 제공하는 것과 직접적으로 관련된 작업에 사용된다. 예를 들어 컨트롤 이벤트를 처리하거나 화면에 그림을 그리는 것이 있다.

## userInitiated

사용자에 의해 명시적으로 요청되었고, 향후 사용자 상호 작용을 허용하기 위해 그 결과가 즉시 나타내어져야 하는 작업을 수행하기 위해 사용된다. 예를 들어 사용자가 메세지 목록에 있는 이메일을 선택한 후 그 이메일을 불러오는 것이 있다.

## default

명시적인 서비스 퀄리티 정보를 알리지 않는다. 가능할 때마다 적절한 서비스 퀄리티가 사용 가능한 소스로부터 결정된다. 그렇지 않으면 `.userInteractive`와 `.utility` 사이의 서비스 퀄리티 수준 정도로 사용된다.

## utility

사용자가 결과를 즉시 기다리지 않아도 되는 작업에 사용된다. 이 작업은 사용자에 의해 요청되었거나 자동으로 초기화될 수 있으며, 종종 모달이 아닌 프로그레스 인디케이터를 사용하여 사용자가 볼 수 있는 타임 스케일에서 동작한다. 예를 들어 주기적인 컨텐츠 업데이트나 미디어 가져오기와 같은 크기가 큰 파일에 대한 작업이 있다.

## background

사용자가 초기하지 않았거나 보이지 않는 작업에 사용된다. 일반적으로 사용자는 이 작업이 일어나고 있는지조차 인지하지 못한다. 예를 들어 컨텐츠를 프리페칭하거나, 색인을 검색하거나, 백업 작업을 하거나, 외부 시스템과 데이터를 동기화화는 것이 있다.