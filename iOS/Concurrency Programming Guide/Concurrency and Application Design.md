# 동시성 및 애플리케이션 설계

컴퓨팅 초기에, 컴퓨터가 수행할 수 있는 시간당 최대 작업량은 CPU의 클록 속도에 의해 결정되었습니다. 하지만 기술이 발전하고 프로세서 설계가 더욱 집약되면서, 열이나 다른 물리적 제약이 프로세서의 최대 클록 속도를 제한하기 시작했습니다. 그래서 칩 제조사들은 칩의 총 성능을 늘리기 위한 다른 방법을 찾았습니다. 그들이 설정한 솔루션은 각 칩에 프로세서 코어의 개수를 늘리는 것이었습니다. 코어의 개수를 늘림으로써, 하나의 칩은 CPU 속도를 향상키시거나, 칩 크기나 열 관련 특성을 변경하지 않고서도 초당 더 많은 명령어를 처리할 수 있었습니다. 유일한 문제는 어떻게 여분의 코어의 이점을 취하느냐는 것이었습니다.

여러 개의 코어에서 오는 이점을 취하기 위해, 컴퓨터는 동시에 여러 개의 일을 할 수 있는 소프트웨어를 필요로 합니다. macOS나 iOS와 같은 현대적인 멀티태스킹 운영체제에서는 주어진 시간에 백 개 혹은 그 이상의 프로그램을 실행할 수 있습니다. 그래서 각각의 프로그램을 다른 코어에 스케줄링하는  것이 가능해야 합니다. 그러나, 이러한 프로그램 중 대부분은 매우 적은 실제 프로세싱 타임을 소비하는 시스템 데몬이거나 백그라운드 애플리케이션입니다. 대신, 정말로 필요한 것은 개별 애플리케이션이 더욱 효과적으로 여분의 코어를 사용하게 하는 방법입니다.

애플리케이션이 여러 개의 코어를 사용하기 위한 전통적인 방법은 여러 개의 쓰레드를 생성하는 것입니다. 그러나, 코어의 개수가 증가하기 때문에 쓰레드 관련 솔루션에 문제가 발생합니다. 가장 큰 문제는 쓰레드 관련 코드가 임의의 코어 개수에 따라 잘 확장되지 않는다는 것입니다. 코어의 개수보다 많은 쓰레드를 생성할 수 없으며 프로그램이 잘 동작할 것이라 기대합니다. 당신이 알 필요가 있는 것은 능률적으로 사용될 수 있는 코어의 개수이며, 이것은 애플리케이션이 계산하기에는 도전적인 것입니다. 당신이 그 개수를 올바르게 얻었다고 할지라도, 매우 많은 쓰레드를 능률적으로 동작하게 하고, 서로 간섭하지 않도록 프로그래밍하는 것은 여전히 도전적입니다.

그러므로 문제를 요약하자면, 애플리케이션이 변동하는 컴퓨터 코어의 개수의 이점을 취하기 위한 방법이 필요하다는 것입니다. 하나의 애플리케이션에 의해 수행되는 작업량 또한 변화하는 시스템 상태에 적응하기 위해 동적으로 조정될 필요가 있습니다. 그리고 그 솔루션은 코어의 이점을 취하기 위해 필요한 작업량을 늘리지 않을 정도로 충분히 단순해야 합니다. 좋은 소식은, Apple의 운영체제가 이러한 모든 문제들에 대한 솔루션을 제공하며, 이 챕터는 이 솔루션을 구성하는 기술과, 이를 활용할 수 있는 코드를 설계할 수 있는 방법을 살펴본다는 것입니다.

## 쓰레드와 멀어지는 움직임

쓰레드는 수년 동안 사용되어 왔고 현재도 사용되고 있으나, 변동 가능한 방식으로 여러 개의 작업을 수행하는 일반적인 문제를 해결해주지 않습니다. 쓰레드를 사용할 때, 변동 가능한 솔루션을 만드는 짐은 개발자의 어깨에 그대로 놓여지게 됩니다. 얼마나 많은 쓰레드를 생성하고 시스템 상태가 변화할 때 동적으로 그 개수를 바꿀지 결정해야 합니다. 애플리케이션이 사용하는 비용의 대부분을 그것이 사용하는 쓰레드를 생성하고 유지하는 것에 사용한다는 것도 또다른 문제입니다.

쓰레드에 의존하는 대신, macOS와 iOS는 동시성 문제를 해결하기 위해 *비동기적 설계 접근법*을 취합니다. 비동기 함수는 수년 동안 운영체제에서 보여져 왔으며, 디스크로부터 데이터를 읽어들이는 것과 같은, 종종 오래 걸리는 작업을 초기화하기 위해 사용됩니다. 비동기 함수가 호출될 때, 작업을 시작하기 위해 씬 뒤에서 어떠한 일을 하지만 실제로 작업이 완료되기 전에 반환합니다. 전형적으로, 이 작업은 백그라운드 쓰레드를 얻는 것을 포함하는데, 그 쓰레드에서 원하는 작업을 시작하며, 작업이 완료되었을 때 호출자에게 알림을 전달하는데, 보통 콜백 함수를 통해 이루어집니다. 과거에는, 비동기 함수가 당신이 하려는 것을 위해 존재하지 않았다면, 이를 위한 비동기 함수를 작성하고 이를 위한 쓰레드를 생성해야 했습니다. 하지만 현재, macOS와 iOS는 당신이 쓰레드를 관리할 필요 없이 어떠한 작업이라도 비동기적으로 수행하게 하는 기술을 제공합니다.

작업을 비동기적으로 시작하기 위한 기술 중 하나는 *Grand Central Dispatch(GCD)*입니다. 이 기술은 일반적으로 당신이 애플리케이션에 작성하는 쓰레드 관리 코드를 시스템 수준으로 내립니다. 당신이 해야 하는 것은 수행하고 싶은 작업을 정의하고 적절한 디스패치 큐에 추가하는 것뿐입니다. GCD가 필요한 쓰레드를 생성하고 이 쓰레드들에 당신의 작업을 스케줄링하는 일을 담당합니다. 쓰레드 관리가 이제 시스템의 부분이 되었기 때문에, GCD는 전통적인 쓰레드보다 더 나은 효율을 제공하면서, 작업 관리와 수행에 전체적인 접근법을 제공합니다.

*Operation Queue*는 Objective-C 오브젝트이며 디스패치 큐와 매우 유사하게 동작합니다. 수행하고 싶은 작업을 정의하고 그것을 오퍼레이션 큐*operation queue*에 추가합니다. 오퍼레이션 큐는 이러한 작업들의 스케줄링과 수행을 처리합니다. GCD처럼, 오퍼레이션 큐는 당신을 위해 모든 쓰레드 관리를 처리하며, 작업들이 시스템에서 가능한 한 빠르게, 능률적으로 수행될 것이라는 것을 보장합니다.

다음의 섹션들은 디스패치 큐, 오퍼레이션 큐, 그리고 다른 관련된 비동기 기술에 대한 더 많은 정보를 제공합니다.

## Dispatch Queues

디스패치 큐는 커스텀 작업을 수행하기 위한 C 기반 메커니즘입니다. 디스패치 큐는 직렬 또는 병렬적으로 작업을 수행하나, 항상 선입선출입니다. 즉, 디스패치 큐는 항상 큐에 추가된 것과 같은 순서로 작업을 큐에서 빼내고 시작합니다. 직렬 디스패치 큐는 한 번에 하나의 작업만을 수행할 수 있으며, 새로운 것을 큐에서 빼내고 시작하기 전 작업이 완료될 때까지 기다립니다. 대조적으로, 병렬 디스패치 큐는 이미 시작된 작업이 끝나기를 기다리지 않고 최대한 많은 작업을 시작합니다.

디스패치 큐는 다음의 이점도 가지고 있습니다.

- 직관적이고 간단한 프로그래밍 인터페이스를 제공합니다.
- 자동화되고 전체적인 쓰레드 풀 관리를 제공합니다.
- 조율된 어셈블리*tuned assembly*의 속도를 제공합니다. 
- 메모리를 훨씬 능률적으로 사용합니다. (쓰레드 스택이 애플리케이션 메모리에 머무르지 않기 때문입니다.)
- 디스패치 큐에 작업을 비동기적으로 디스패치하는 것은 큐에 데드락 현상을 일으키지 않습니다.
- 투쟁 하에서 우아하게 스케일링됩니다.
- 직렬 디스패치 큐는 lock과 다른 원시적인 동기화 기법보다 더 능률적인 대안을 제공합니다.

당신이 디스패치 큐에 제출하는 작업은 함수나 블록 오브젝트 안에 캡슐화되어야 합니다. *블록 오브젝트*는 C언어의 기능이며 개념적으로 함수 포인터와 비슷하나 추가적인 이점을 가지고 있습니다. 그 자신의 어휘적 스코프 내에 블록을 정의하는 대신, 또다른 함수나 메소드 안에 있는 블록을 정의하여 그 함수나 메소드로부터 다른 변수에 접근할 수 있습니다. 블록은 또한 기존 스코프 밖으로 이동하여 힙에 복사될 수 있습니다. 이는 당신이 그것들을 디스패치 큐에 제출할 때 발생하는 것입니다. 이러한 모든 의미는 상대적으로 적은 양의 코드로 매우 동적인 작업을 구현할 수 있다는 것입니다.

디스패치 큐는 Grand Central Dispatch 기술의 일부분이며 C 런타임의 일부분입니다. 애플리케이션에서 디스패치 큐를 사용하는 것에 대한 더 많은 정보를 Dispatch Queues에서 확인하십시오. 블록과 그 이점에 대한 더 많은 정보를 Blocks Programming Topics에서 확인하십시오.

## Dispatch Sources

디스패치 소스는 특정 시스템 이벤트 타입을 비동기적으로 처리하기 위한 C 기반 메커니즘입니다. 디스패치 소스는 특정 타입의 시스템 이벤트에 대한 정보를 캡슐화하고, 그 이벤트가 일어날 때마다 특정 블록 오브젝트나 함수를 디스패치 큐에 제출합니다. 다음의 시스템 이벤트를 모니터링하기 위해 디스패치 소스를 사용할 수 있습니다.

- 타이머
- 시그널 핸들러
- 디스크립터 관련 이벤트
- 프로세스 관련 이벤트
- Mach port 이벤트
- 당신이 촉발하는 커스텀 이벤트

디스패치 소스는 Grand Central Dispatch 기술의 일부분입니다. 애플리케이션에서 이벤트를 받기 위해 디스패치 소스를 사용하는 것에 대한 더 많은 정보를 Dispatch Sources에서 확인하십시오.

## Operation Queues

 큐는 동시적 디스패치 큐의 Cocoa 프레임워크 구현이며 `NSOperationQueue` 클래스로 구현되어 있습니다. 디스패치 큐가 항상 작업들을 선입선출로 수행하는 반면, 오퍼레이션 큐는 작업의 수행 순서를 결정할 때 다른 요소를 고려합니다. 이러한 요소 중 주요한 것은 주어진 작업이 다른 작업의 완료에 의존하는지에 대한 여부입니다. 당신은 작업을 정의할 때 의존성을 구성하고, 복잡한 수행 순서 그래프를 생성하기 위해 사용할 수 있습니다.

당신이 오퍼레이션 큐에 제출하는 작업들은 `NSOperation` 클래스의 인스턴스여야 합니다. *오퍼레이션 오브젝트*는 수행하기 원하는 작업과 그것을 수행하는데 필요한 어떠한 데이터를 캡슐화한 Objective-C 오브젝트입니다. `NSOperation` 클래스가 본질적으로 추상화된 기반 클래스이기 때문에, 일반적으로 당신의 작업을 수행하기 위해 커스텀 서브클래스를 정의합니다. 하지만 Foundation 프레임워크에는 작업을 수행하기 위해 생성하고 사용할 수 있는 몇몇 구체적인 서브클래스를 포함합니다.

오퍼레이션 오브젝트는 키-값 옵저빙(KVO) 알림을 생성하며, 이는 작업의 진행 상황을 모니터링하는 유용한 방법이 될 수 있습니다. 오퍼레이션 큐가 항상 동시적으로 오퍼레이션을 수행한다 할지라도, 그것들이 필요할 때 직렬적으로 수행된다는 것을 보장하기 위해 의존성을 사용할 수 있습니다.

오퍼레이션 큐를 사용하는 방법, 커스텀 오퍼레이션 오브젝트를 정의하는 방법에 대한 더 많은 정보를 Operation Queues에서 확인하십시오.

## 비동기적 설계 테크닉

당신의 코드가 동시성을 지원하도록 다시 설계하는 것을 고려하기 전에, 이것이 정말로 필요한 것인지 자신에게 질문해야 합니다. 동시성은 메인 쓰레드가 사용자 이벤트에 응답하기 위해 풀려 있다는 것을 보장하여 코드의 응답성을 향상시킬 수 있습니다. 심지어 동일한 시간에 더 많은 작업을 하기 위해 더 많은 코어를 사용하여 코드의 능률을 향상시킬 수 있습니다. 하지만, 오버헤드를 추가하고 코드의 전체적인 복잡성을 증가시켜, 코드를 작성하고 디버깅하기 어렵게 하기도 합니다.

그것이 복잡성을 더하기 때문에, 동시성은 프로덕트 사이클의 마지막에 접목할 수 있는 특징이 아닙니다. 이를 올바르게 하는 것은 애플리케이션이 수행하는 작업들과 그러한 작업을 수행하기 위해 사용되는 자료구조에 대해 사려 깊은 고민을 요구합니다. 올바르지 않게 되어싿면, 이전보다 더 느려진 코드를 발견할 것이고 유저와의 응답성도 낮아질 것입니다. 그러므로, 몇 가지 목표를 설정하기 위해 설계 사이클 초반에 시간을 들이고 당신이 취할 필요가 있는 접근법에 대해 생각하는 것은 매우 가치 있는 일입니다.

모든 애플리케이션은 다른 요구사항과 그것이 수행하는 다른 작업 집합을 가지고 있습니다. 애플리케이션과 그것과 관련된 작업을 설계하는 방법을 정확하게 알려주기는 불가능합니다. 하지만 이어지는 섹션은 설계 프로세스 동안 좋은 선택을 하는 것을 돕는 몇 가지 가이드라인을 제시할 것입니다.

### 애플리케이션의 기대 행동 정의하기

애플리케이션에 동시성을 추가하는 것에 대해 생각하기 전에, 애플리케이션의 올바른 행동이 무엇인지 정의하는 것으로 시작해야 합니다. 애플리케이션의 기대되는 행동을 이해하는 것은 이후에 설계를 검증할 방법을 제공합니다. 또한 동시성을 도입함으로써 얻을 수 있는 기대되는 퍼포먼스 이점에 대한 아이디어도 받을 수 있습니다.

가장 먼저 해야 할 것은 애플리케이션이 수행하는 작업과 각각의 작업과 관련된 오브젝트나 자료구조를 을 열거해놓는 것입니다. 초기에, 사용자가 메뉴 아이템을 선택하거나 버튼을 클릭할 때 작업이 수행되기를 원할 수 있습니다. 이러한 작업들은 별개의 행동을 제공하며 시작점과 끝점이 잘 정의되어 있습니다. 당신은 타이머 기반 작업과 같은, 사용자 인터랙션 없이 수행되는 다른 타입의 작업에 대해서도 열거해야 합니다.

고수준 작업을 나열해 놓았다면, 성공적으로 해당 작업을 완료하기 위해 취해져야 하는 단계의 집합으로 각각의 작업을 쪼개기 시작하십시오. 이 단계에서, 당신은 우선적으로 어떠한 자료구조 및 오브젝트를 만들 필요가 있는 수정 사항에 대해 생각해야 하며, 이러한 수정이 애플리케이션의 전체 상태에 어떤 영향을 미치는지 생각해야 합니다. 또한 오브젝트와 자료구조 간 의존성도 생각해야 합니다. 예를 들어, 오브젝트를 담은 배열에 같은 변화를 일으키는 작업을 포함한다면, 하나의 오브젝트의 변화가 다른 어떠한 오브젝트에 영향을 미치는지에 대해 알아두는 것은 중요합니다. 만약 오브젝트들이 서로 독립적으로 수정할 수 있다면, 그것은 동시적으로 이러한 수정을 일으킬 수 있는 곳일 것입니다.

### 실행 가능한 작업 단위 알아내기

애플리케이션의 작업에 대한 이해에서, 당신은 코드가 동시성으로부터 이점을 얻을 수 있는 장소를 진작에 식별할 수 있어야 합니다. 하나 또는 그 이상의 작업 단계 순서의 변화가 결과를 변화시킨다면, 이러한 단계 수행을 직렬적으로 할 필요가 있을 것입니다. 순서의 변화가 출력에 영향을 주지 않는다면, 동시적으로 이러한 단계를 수행하는 것을 고려할 수 있을 것입니다. 두 가지 모두에서, 당신은 해당 단계나 단계들이 수행되는 것을 나타내는 실행 가능한 작업 단위를 정의합니다. 이 작업 단위는 블록이나 오퍼레이션 오브젝트를 사용하여 캡슐화하고 적절한 큐에 디스패치하는 것이 됩니다.

### 필요로 하는 큐 식별하기

당신의 작업들은 개별의 작업 단위로 쪼개어져서 블록 오브젝트나 오퍼레이션 오브젝트를 사용하여 캡슐화되었습니다. 이제는 그 코드를 실행하기 위해 사용할 큐를 정의할 필요가 있습니다. 주어진 작업에 대해서, 당신이 생성한 블록이나 오퍼레이션 오브젝트를 심사하고, 작업을 올바르게 수행하기 위해 실행되어야 하는 순서를 심사하십시오.

블록을 사용하여 작업을 구현한다면, 블록을 직렬 또는 병렬 디스패치 큐에 추가할 수 있습니다. 특정한 순서를 필요로 한다면, 항상 직렬 디스패치 큐에 블록을 추가할 것입니다. 특정 순서를 요구하지 않는다면, 병렬 디스패치 큐에 블록을 추가하거나, 니즈에 따라서는 몇몇 다른 디스패치 큐에 그것들을 추가할 수 있습니다.

오퍼레이션 오브젝트를 사용하여 작업을 구현한다면, 큐를 선택하는 것은 오브젝트를 구성하는 것보다 관심이 덜 가는 부분이 됩니다. 직렬로 오퍼레이션 큐를 수행하기 위해, 관련된 오브젝트 간 의존성을 구성해야 합니다. 의존성은 의존하는 어떠한 오브젝트가 그 작업을 완료할 때까지 이에 의존하는 오퍼레이션 실행을 방지합니다.

### 능률 향상을 위한 팁

코드를 더 작은 작업으로 쪼개고 그것들을 큐에 추가하는 것에 더하는 것뿐만 아니라, 큐를 사용하여 코드의 전체적인 능률을 향상시킬 수 있는 다른 방법이 있습니다.

- 메모리 사용량이 요소일 때, 작업 내에서 값 연산을 직접적으로 하는 것을 고려하십시오. 애플리케이션이 이미 메모리를 많이 사용하고 있다면, 직접적으로 값을 연산하는 것은 메인 메모리로부터 캐시된 값을 불러오는 것보다 더 빠를 수 있습니다. 직접적으로 값을 연산하는 것은 주어진 프로세서 코어의 레지스터와 케시를 사용하며, 이는 메인 메모리보다 훨씬 빠릅니다. 물론 테스트에서 이것이 더 좋은 퍼포먼스를 낸다는 것을 나타낼 때만 이렇게 해야 합니다.
- 직렬 작업을 식별하고 그것들을 더 병렬적으로 만들기 위해 할 수 있는 것을 하십시오. 어떠한 작업이 몇몇 공유 리소스에 의존하기 때문에 직렬적으로 실행되어야 한다면, 공유 리소스를 지우기 위해 아키텍쳐를 변경하는 것을 고려하십시오. 리소스를 필요로 하거나 리소스를 모두 제거하는 각 클라이언트에 대하여 리소스의 복사본을 만드는 것을 고려할 수 있습니다.
- 락*lock*을 사용하지 마십시오. 디스패치 큐와 오퍼레이션 큐가 제공하는 지원은 대부분의 상황에서 락을 불필요하게 만듭니다. 몇몇 공유 리소스를 보호하기 위해 락을 사용하는 것 대신, 올바른 순서로 작업을 실행하기 위해 직렬 큐를 지정하거나 오퍼레이션 오브젝트 의존성을 사용하십시오.
- 가능하다면 시스템 프레임워크에 의존하십시오. 동시성을 얻는 가장 좋은 방법은 시스템 프레임워크가 제공하는 내장 동시성의 이점을 취하는 것입니다. 많은 프레임워크는 동시성 행동을 구현하기 위해 쓰레드 및 다른 기술을 내부적으로 사용합니다. 작업을 정의할 때, 존재하는 프레임워크가 당신이 원하는 것과 정확한 일을 하며 동시적으로 동작하는 함수나 메소드를 정의하고 있는지 확인하십시오. 그 API를 사용하여 당신의 노력을 줄여줄 수 있고 가능한 한 최대의 동시성을 제공할 것입니다.

## 퍼포먼스 함축

오퍼레이션 큐, 디스패치 큐, 디스패치 소스는 당신이 더 많은 코드를 동시적으로 실행하는 것을 쉽게 하기 위해 제공됩니다. 그러나, 이러한 기술들은 애플리케이션의 응답성이나 능률에 대한 성능 향상을 보장하지 않습니다. 큐를 적절하게 사용하여 당신의 니즈에 맞게 효과적으로, 애플리케이션의 다른 리소스에 불필요한 짐을 지게 하지 않게 하는 것은 여전히 당신에게 달려 있습니다. 예를 들어, 당신이 1만 개의 오퍼레이션 오브젝트를 생성하고 그것들을 어떠한 오퍼레이션 큐에 제출할 수 있다고 할지라도, 그렇게 하는 것은 애플리케이션이 잠재적으로 사소하지 않은 양의 메모리를 할당하는 원인이 되며, 이는 페이징 및 퍼포먼스 하락의 결과를 가져올 수 있습니다.

큐를 사용하든 쓰레드를 사용하든, 당신의 코드에 어떠한 동시성을 도입하기 전에, 애플리케이션의 현재 퍼포먼스를 반영하는 기준 수치 집합을 모아야 할 필요가 있습니다. 변화를 도입한 후 추가 수치를 모으고 기준과 비교하여 애플리케이션의 전체 능률이 향상되었는지 확인해야 합니다. 동시성의 도입이 애플리케이션의 능률 및 응답성을 하락시켰다면, 잠재 원인을 확인하기 위해 사용 가능한 퍼포먼스 툴을 사용해야 합니다.

퍼포먼스에 대한 소개 및 사용 가능한 퍼포먼스 툴, 더 나아간 퍼포먼스 관련 토픽을 확인하기 위해 Performance Overview를 참고하십시오.

## 동시성 및 다른 기술

코드를 모듈화된 작업들로 만드는 것은 애플리케이션에서 동시성의 양을 시도하고 향상시키는 가장 좋은 방법입니다. 하지만, 이 설계 접근법은 모든 상황에서 모든 애플리케이션의 니즈를 만족시켜 주지 않습니다. 당신의 작업들에 따라, 애플리케이션의 전체적인 동시성에 대하여 추가 향상을 제공할 수 있는 다른 선택지가 있을 수 있습니다. 이 섹션은 설계 부분에서 사용하기를 고려할만한 다른 기술 중 몇 개에 대한 개요입니다.

### OpenCL 및 동시성

macOS에서 *Open Computing Language(OpenCL)*은 컴퓨터의 그래픽 프로세서에서 범용 연산을 수행하기 위한 기초 기반 기술입니다. OpenCL은 거대한 데이터셋을 적용하기 원하는 잘 정의된 연산 집합을 가지고 있다면 사용하기 좋은 기술입니다. 예를 들어, 이미지의 픽셀에서 필터링 연산을 수행하거나 한번에 여러 개의 값에 대한 복잡한 수학적 연산을 수행하는 데 OpenCL을 사용할 수 있을 것입니다. 즉, OpenCL은 데이터를 병렬적으로 조작할 수 있는 문제 집합들에 더욱 중점을 두어 설계되었습니다.

OpenCL이 거대한 데이터 병렬 오퍼레이션을 수행하기 좋다고 할지라도, 더욱 범용적인 연산에 대해서는 적합하지 않습니다. GPU에 의해 연산되게 하기 위해 데이터와 요구되는 작업 커널 모두를 준비하고 그래픽 카드로 전송하기 위해 요구되는 상당한 양의 노력이 있습니다. 비슷하게, OpenCL에 의해 생성된 어떠한 결과를 받기 위해 요구되는 상당한 양의 노력이 있습니다. 결과적으로, 시스템과 상호작용하는 작업들은 일반적으로 OpenCL을 사용하는 것을 추천하지 않습니다. 예를 들어, 파일 또는 네트워크 스트림으로부터 데이터를 처리하는 것에 OpenCL을 사용하지 않을 것입니다. 대신, OpenCL을 사용하여 수행하는 작업은 그래픽 프로세서로 전송되고 독립적으로 연산될 수 있도록 훨씬 더 독립적인 것이어야 합니다.

OpenCL 및 사용 방법에 대한 더 많은 정보를 OpenCL Programming Guide for Mac에서 참고하십시오.

### 쓰레드를 사용하는 때

오퍼레이션 큐 및 디스패치 큐가 동시적으로 작업을 수행하기 위해 선호되는 방법이라 할지라도, 이것은 만병통치약이 아닙니다. 애플리케이션에 따라 여전히 커스텀 쓰레드를 생성할 필요가 있는 때가 여전히 있을 것입니다. 커스텀 쓰레드를 생성한다면, 가능한 한 적은 쓰레드를 생성하기 위해 노력해야 하고, 다른 어떠한 방법으로도 구현될 수 없는 특정 작업을 위해서만 그러한 쓰레드를 사용해야 합니다.

쓰레드는 반드시 실시간으로 동작해야 하는 코드를 구현하기 위한 여전히 좋은 방법입니다. 디스패치 큐는 가능한 한 빠르게 그들이 작업을 실행하기 위한 시도를 하지만 실시간 제약을 전달하지 않습니다. 백그라운드에서 동작하는 코드로부터 더욱 예측 가능한 행동을 필요로 한다면, 쓰레드는 여전히 더 나은 대안을 제공해줄 것입니다.

다른 쓰레드 프로그래밍과 마찬가지로, 절대적으로 필요한 경우에만 항상 쓰레드를 적절하게 사용해야 합니다. 쓰레드 패키지 및 그 사용법에 대한 자세한 정보를 Threading Programming Guide에서 참고하십시오.