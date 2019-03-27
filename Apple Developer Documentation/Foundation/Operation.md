# Operation

**클래스** | 단일 작업과 관련된 코드 및 데이터를 나타내는 추상 클래스

---

## 개요

`Operation` 클래스는 추상 클래스이기 때문에, 이것을 직접 사용하는 대신 서브클래싱하거나 시스템이 정의한 서브클래스(`BlockOperation` 등)를 사용하여 실제 작업을 수행한다. 이것이 추상화되어 있을지라도, `Operation`의 기반 구현은 당신의 작업에서 안전한 실행을 조정하기 위한 중요한 로직을 포함한다. 내장 로직의 존재는 당신이 어떠한 작업이 다른 시스템 오브젝트들과 함께 올바르게 작동하는 것을 확신하기 위해 필요한 코드들 대신에, 작업의 실제 구현에만 집중할 수 있게 해준다.

오퍼레이션 오브젝트는 single-shot 오브젝트이다. 즉, 그것은 그 작업을 한번만 수행하고 다시는 수행하는데 사용되지 않는다. 당신은 전형적으로 오퍼레이션 큐(`OperationQueue` 클래스의 인스턴스)에 오브젝트들을 추가하여 오퍼레이션들을 실행한다. 오퍼레이션 큐는 부차적인 쓰레드에서 동작하게 함으로써 직접적으로 오퍼레이션들을 실행하거나, `libdispatch` 라이브러리(Grand Central Dispatch라고 알려져 있음)를 사용하여 간접적으로 실행한다.

오퍼레이션 큐를 사용하기 원하지 않는다면, 코드에서 `start()` 메소드를 직접 호출하여 오퍼레이션을 실행할 수 있다. 오퍼레이션을 수동으로 실행하는 것은 코드에 더 많은 짐을 지어주게 되는데, 준비 상태에 있지 않은 오퍼레이션을 시작하는 것은 예외를 발생시키기 때문이다. `isReady` 프로퍼티는 오퍼레이션의 준비 상태에 대해 보고한다.

## 오퍼레이션 의존성

의존성은 특정 순서로 오퍼레이션들을 실행하기 위한 편리한 방법이다. `addDependency(_:)` 또는 `removeDependency(_:)` 메소드를 사용하여 오퍼레이션에 의존성을 추가하거나 제거할 수 있다. 기본적으로, 의존성을 가진 오퍼레이션 오브젝트는 그것이 의존하는 모든 오퍼레이션 오브젝트가 실행을 완료할 때까지 준비되지 않은 것으로 간주된다. 하지만, 일단 마지막 의존 오퍼레이션의 실행이 완료되면, 오퍼레이션 오브젝트는 준비된 상태가 되어 실행 가능한 상태가 된다.

`NSOperation`에 의해 지원되는 의존성은 의존하는 오퍼레이션이 성공적으로 또는 성공하지 못하고 완료되었는지에 대한 구별을 하지 않는다. (즉, 오퍼레이션을 취소하는 것도 그것이 완료된 것과 비슷하게 표시된다.) 의존성을 가진 오퍼레이션이 그것이 의존하는 오퍼레이션들이 취소되었거나 성공적으로 작업을 완료하지 못한 경우에 계속 진행할 것인지에 대해 결정하는 것은 당신에게 달려 있다. 이는 오퍼레이션 오브젝트에 몇 가지 추가적인 에러 트래킹 기능을 통합해야 할 필요를 가져다 준다.

## KVO를 준수하는 프로퍼티

`NSOperation` 클래스는 몇 가지 프로퍼티에 대해 키-값 코딩(KVC) 및 키-값 옵저빙(KVO)을 지원한다. 필요하다면 다음의 프로퍼티들을 관찰하여 애플리케이션의 다른 부분에서 통제할 수 있다. 다음의 키 경로를 사용해서 프로퍼티를 관찰할 수 있다.

- `isCancelled` : 읽기 전용
- `isAsynchronous` : 읽기 전용
- `isExecuting` : 읽기 전용
- `isFinished` : 읽기 전용
- `isReady` : 읽기 전용
- `dependencies` : 읽기 전용
- `queuePriority` : 읽기 및 쓰기 가능
- `completionBlock` : 읽기 및 쓰기 가능

당신이 이러한 프로퍼티에 옵저버를 부착할 수 있을지라도, 애플리케이션의 UI 요소에 그것들을 바인딩하기 위해 코코아 바인딩을 사용해서는 안된다. UI와 관련된 코드는 전형적으로 애플리케이션의 메인 쓰레드에서만 실행되어야 한다. 오퍼레이션은 어느 쓰레드에서도 실행될 수 있기 때문에, 그 오퍼레이션과 관련된 KVO 알림도 어느 쓰레드에서도 일어날 수 있다.

위의 프로퍼티에 커스텀 구현을 제공한다면, KVC 및 KVO 준수를 유지하는 구현을 해야 한다. `NSOperation` 오브젝트에 추가 프로퍼티를 정의한다면, 또한 그 프로퍼티들도 KVC 및 KVO 알림을 준수하는 것을 추천한다.

## 멀티 코어 고려사항

`NSOperation` 클래스는 그 자체가 멀티 코어를 인지한다. 그러므로 오브젝트에 대한 접근을 동기화하기 위해 추가적인 락을 생성하지 않고 여러 개의 쓰레드에서 `NSOperation` 오브젝트의 메소드를 호출하는 것은 안전하다. 이 행동은 오퍼레이션이 전형적으로 그것을 생성하고 모니터링하는 쓰레드와는 개별적인 쓰레드에서 동작하기 때문에 필수적이다.

`NSOperation` 클래스를 서브클래싱할 때, 어떠한 재정의된 메소드가 여러 개의 쓰레드에서의 호출에 안전한 것을 유지하는지를 보장해야 한다. 서브클래스에서 커스텀 데이터 접근자와 같은 커스텀 메소드를 구현한다면, 그러한 메소드들도 쓰레드에 안전하다는 것을 보장해야 한다. 그러므로, 오퍼레이션에 있는 어떠한 데이터 변수에 접근하는 것은 데이터 훼손의 가능성을 없애기 위해 동기화되어야 한다.

## 비동기 vs. 동기 오퍼레이션

수동으로 오퍼레이션 오브젝트를 실행할 계획을 가지고 있다면, 그것을 큐에 추가하는 대신, 동기 혹은 비동기 방식으로 오퍼레이션이 실행되도록 설계할 수 있다. 오퍼레이션 오브젝트들은 기본적으로 동기적이다. 동기 오퍼레이션에서, 오퍼레이션 오브젝트는 그 작업을 실행하는 쓰레드와 개별적인 쓰레드를 생성하지 않는다. 코드에서 직접적으로 동기 오퍼레이션의 `start()` 메소드를 호출할 때, 현재 쓰레드에서 즉시 오퍼레이션을 실행한다. 그러한 오브젝트의 `start()` 메소드가 제어권을 호출자에게 반환할 때, 작업 자체는 완료된다.

비동기 오퍼레이션의 `start()` 메소드를 호출할 때, 그 메소드는 이에 대응하는 작업이 완료되기 전에 반환될 수 있다. 비동기 오퍼레이션 오브젝트는 개별 쓰레드에 그 작업을 스케줄링할 책임이 있다. 오퍼레이션은 직접 새로운 쓰레드를 시작하여, 비동기 메소드를 호출하여, 디스패치 큐에 블록을 제출하여 그렇게 할 수 있다. 제어권이 호출자에게 반환될 때 오퍼레이션이 실행 중인지 아닌지는 실제로 중요하지 않으며, 단지 그것이 실행 중일 수 있다는 것이 중요하다.

오퍼레이션을 실행하기 위해 항상 큐를 사용하려 한다면, 비동기적으로 그것들을 정의하는 것은 더 단순하다. 그럼에도 불구하고, 수동으로 오퍼레이션을 실행한다면, 오퍼레이션 오브젝트를 비동기적으로 정의하는 것을 원할 것이다. 비동기 오퍼레이션을 정의하는 것은 더 많은 작업을 필요로 하는데, KVO 알림을 사용하여 작업의 진행 상태를 모니터링하고 상태 변화를 보고해야 하기 때문이다. 하지만 비동기 오퍼레이션을 정의하는 것은 수동으로 실행된 오퍼레이션이 호출 쓰레드를 가로막지 않는 것을 확신하기 위한 경우에 유용하다.

오퍼레이션을 오퍼레이션 큐에 추가할 때, 큐는 `isAsynchronous` 프로퍼티의 값을 무시하고 개별 쓰레드로부터 항상 `start()` 메소드를 호출한다. 그러므로, 항상 오퍼레이션 큐에 그것들을 추가하여 오퍼레이션을 실행한다면, 그것들을 비동기적으로 만들 이유가 없다.

## 서브클래싱 관련 알아두기

`NSOperation` 클래스는 오퍼레이션의 실행 상태를 트래킹하기 위한 기본 로직을 제공하나 실제 작업을 하기 위해서는 서브클래싱되어야 한다. 서브클래스를 생성하는 방법은 오퍼레이션이 동시적으로 실행되는가, 동시적이지 않게 실행되는가 지정하는 것에 달려 있다.

### 재정의 메소드

동시적이지 않은 오퍼레이션에 대하여, 일반적으로 오직 하나의 메소드만을 재정의하면 된다.

- `main()`

이 메소드에서, 주어진 작업을 수행하기 위해 필요한 코드를 위치시킨다. 물론 커스텀 클래스의 인스턴스를 생성하는 것을 더욱 쉽게 하기 위한 커스텀 초기화 메소드를 정의할 수도 있다. 오퍼레이션에서 데이터에 접근하기 위한 접근자 및 설정자 메소드를 정의하기 원할 수도 있다. 그러나, 커스텀 접근자 및 설정자 메소드를 정의한다면, 이러한 메소드들이 여러메소드에서 안전하게 호출되는 것을 보장해야 한다.

동시 오퍼레이션을 생성한다면, 최소한 다음의 메소드와 프로퍼티를 재정의해야 한다.

- `start()`
- `isAsynchronous`
- `isExecuting`
- `isFinished`

동시 오퍼레이션에서, `start()` 메소드는 비동기적 방식에서 오퍼레이션을 시작할 책임이 있다. 쓰레드를 생성하든 비동기 함수를 호출하든, 그것은 이 메소드에서 이루어져야 한다. 오퍼레이션을 시작한 이후, `start()` 메소드는 `isExecuting` 프로퍼티가 보고하는 오퍼레이션의 실행 상태도 갱신해야 한다. `isExecuting` 키 경로에 대한 KVO 알림을 전송하여 이를 할 수 있는데, 관심 있어 하는 클라이언트가 그 오퍼레잇녀이 실행 중인지 알게 할 수 있다. `isExecuting` 프로퍼티는 또한 쓰레드 안전한 방식에서 상태를 제공해야 한다.

작업의 완료 또는 취소 이후에, 동시 오퍼레이션 오브젝트는 오퍼레이션의 최종 변화 상태를 표시하기 위해 `isExecuting` 및 `isFinished` 키 경로에 대한 KVO 알림을 생성해야 한다. (취소의 경우, `isFinished` 키 경로를 갱신하는 것은, 작업이 완전하게 완료되지 않을지라도 여전히 중요하다. 대기 중인 오퍼레이션은 그것들이 큐에서 제거되기 전에 완료되었다고 보고해야 한다.) KVO 알림을 생성하는데 더하여, `isExecuting` 및 `isFinished` 프로퍼티를 재정의하여 오퍼레이션 상태에 기반한 정확한 값을 보고하는 것을 지속해야 한다.

> **중요**
>
> `start()` 메소드는 `super`를 호출할 일이 없어야 한다. 동시 오퍼레이션을 정의할 때, 기본 `start()` 메소드가 제공하는 것과 같은 행동을 제공하는 것은 당신에게 달려 있으며, 이는 작업을 시작하고 적절한 KVO 알림을 생성하는 것을 포함한다. `start()` 메소드는 또한 오퍼레이션 자체가 실제로 작업이 시작되기 전 취소되었는지에 대해 확인해야 한다.

동시 오퍼레이션에 대해서도, 위에서 기술한 것 이외의 메소드를 재정의할 필요는 거의없다. 하지만, 오퍼레이션의 의존성 기능을 커스터마이징한다면, 추가 메소드를 재정의하고 추가 KVO 알림을 제공해야 할 수 있다. 의존성의 경우, 오직 `isReady` 키 경로에 대한 알림을 제공하는 것만을 요구한다. `dependencies` 프로퍼티는 의존하는 오퍼레이션의 목록을 포함하므로, 그것의 변화는 이미 기본 `NSOperation` 클래스에 의해 처리된다.

### 오퍼레이션 오브젝트의 상태 관리하기

오퍼레이션 오브젝트는 그것이 실행하기에 안전한지 결정하기 위해 내부적으로 상태 정보를 관리하며, 오퍼레이션의 생명 주기를 통하여 진행 상황에 대하여 외부 클라이언트에게 통지한다. 커스텀 서브클래스는 이 상태 정보를 관리하여 코드에서 올바른 실행을 하고 있는지 보장해야 한다. 오퍼레이션의 상태와 관련된 키 경로는 다음과 같다.

#### isReady

`isReady` 키 경로는 클라이언트들이 어떠한 오퍼레이션이 실행될 준비가 되어있을 때 알도록 한다. `isReady` 프로퍼티는 그 오퍼레이션이 실행될 준비가 되어 있으면 `true`를 반환하고, 그것이 의존하는 오퍼레이션들이 여전히 종료되지 않은 상태라면 `false`를 반환한다.

대부분의 경우, 이 키 경로에 대한 상태를 당신이 관리할 필요는 없다. 그러나, 오퍼레이션의 준비 상태가 프로그램의 어떠한 외부 상태와 같은, 의존하는 오퍼레이션 이외의 요소로 결정된다면, `isReady` 프로퍼티에 대한 당신 자신의 구현을 제공하고 그 오퍼레이션의 준비 상태를 트래킹하게 할 수 있다. 외부 상태가 그것을 허용하는 경우에만 오퍼레이션 오브젝트를 생성하는 것이 더 간단할 수도 있다.

오퍼레이션이 하나 또는 그 이상의 의존하는 오퍼레이션들의 완료를 기다리는 동안에 취소된다면, 그것들의 의존성은 무시되며 이 프로퍼티의 값은 그것이 이제 실행될 준비가 되었다는 것을 반영하기 위해 갱신된다. 이 행동은 오퍼레이션 큐가 취소된 오퍼레이션을 큐에서 빠르게 빼내는 것을 가능하게 한다.

#### isExecuting

`isExecuting` 키 경로는 클라이언트들이 어떠한 오퍼레이션이 할당된 작업을 활발하게 수행하고 있다는 것을 알게 한다. `isExecuting` 프로퍼티는 오퍼레이션이 그 작업을 수행하고 있다면 `true`를 보고해야 하고 그렇지 않으면 `false`를 반환해야 한다.

오퍼레이션 오브젝트의 `start()` 메소드를 교체한다면, `isExecuting` 프로퍼티도 교체하고 오퍼레이션의 실행 상태가 변화할 때 KVO 알림을 생성해야 한다.

#### isFinished

`isFinished` 키 경로는 클라이언트들이 어떠한 오퍼레이션이 그 작업을 성공적으로 완료하거나 취소하였고, 종료되었는지 알게 한다. 오퍼레이션 오브젝트는 `isFinished` 키 경로의 값이 `true`로 변하기 전까지 의존성을 지우지 않는다. 비슷하게, 오퍼레이션 큐는 오퍼레이션의 `isFinished` 프로퍼티가 `true` 값을 가질 때까지 그것을 큐에서 빼내지 않는다. 그러므로, 오퍼레이션을 완료된 것으로 표시하는 것은 진행 중 또는 취소된 오퍼레이션으로 큐가 백업되지 않도록 하는 데 중요하다.

`start()` 메소드 또는 오퍼레이션 오브젝트를 교체하지 않는다면, `isFinished` 프로퍼티도 교체하고 오퍼레이션이 실행을 완료하였거나 취소했을 때 KVO 알림을 생성하도록 해야 한다.

#### isCancelled

`isCancelled` 키 경로는 클라이언트들이 오퍼레이션의 취소가 요청되었음을 알게 한다. 취소에 대한 지원은 자원적이나 장려되며, 코드는 이 키 경로에 대한 KVO 알림을 전송하지 않을 필요가 있다.

### 취소 명령에 응답하기

일단 오퍼레이션을 큐에 추가하고 나면, 오퍼레이션은 당신의 손을 떠나게 된다. 큐가 작업의 스케줄링 책임을 떠맡고 처리한다. 그러나, 예를 들어 사용자가 진행 패널에서 취소 버튼을 누르거나 애플리케이션을 종료하여 나중에 오퍼레이션을 진행하기 원하지 않을 경우, 불필요한 CPU 시간을 소비하지 않게 하기 위해 오퍼레이션을 취소할 수 있다. 오퍼레이션 오브젝트 자체에서 `cancel()` 메소드를 호출하거나, `OperationQueue` 클래스의 `cancelAllOperations()` 메소드를 호출하여 이를 할 수 있다.

오퍼레이션을 취소하는 것은 그것이 하고 있는 것을 즉시 중단하는 것을 강제하지 않는다. `isCancelled` 프로퍼티에 있는 값이 반영하는 것이 모든 오퍼레이션에 대한 것이라고 기대할지라도, 코드는 명시적으로 이 프로퍼티의 값을 확인하고 필요하다면 중단해야 한다. `NSOperation`의 기본 구현은 취소에 대한 확인을 포함한다. 예를 들어, `start()` 메소드를 호출하기 전에 오퍼레이션을 취소한다면, `start()` 메소드는 작업을 시작하지 않고 종료된다.

> 오퍼레이션 큐에 들어가 있고 이것이 의존하는 오퍼레이션들이 종료되지 않은 상태에서 `cancel()` 메소드를 호출한다면, 그것들이 의존하는 오퍼레이션들은 무시된다. 오퍼레이션이 이미 취소되었기 때문에, 이 행동은 `main()` 메소드를 호출하지 않고 큐에서 오퍼레이션을 제거하기 위해 `start()` 메소드를 호출하는 것을 가능하게 한다. 큐에 있지 않은 오퍼레이션에서 `cancel()` 메소드를 호출한다면, 오퍼레이션은 즉시 취소된 것으로 표시된다. 각각의 경우에, 오퍼레이션을 준비 또는 완료된 상태로 표시하는 것은 적절한 KVO 알림의 생성의 결과를 가져온다.

당신이 작성하는 커스텀 코드에서 항상 취소 의미론을 지원해야 한다. 특히, 주요 작업 코드가 주기적으로 `isCancelled` 프로퍼티의 값을 확인해야 한다. 그 프로퍼티가 `true` 값을 보고한다면, 오퍼레이션 오브젝트는 가능한 한 빠르게 정리되고 종료되어야 한다. 커스텀 `start()` 메소드를 구현한다면, 그 메소드는 취소 상태에 대해 일찍이 확인하는 것을 포함하고 적절하게 행동해야 한다. 커스텀 `start()` 메소드는 이른 취소에 대한 타입을 처리하기 위해 준비되어야 한다.

오퍼레이션이 취소될 때 단순하게 빠져나가는 것에 더하여, 취소된 오퍼레이션을 적절한 최종 상태로 옮기는 것도 중요하다. 특히, 당신이 `isFinished` 및 `isExecuting` 프로퍼티에 대한 값을 관리한다면 (아마 동시 오퍼레이션을 구현하기 위해), 이에 따라서 그러한 프로퍼티들을 갱신해야 한다. 특히, `isFinished`가 반환하는 값을 `true`로, `isExecuting`이 반환하는 값을 `false`로 바꾸어야 한다. 오퍼레이션이 실행을 시작하기 전에 취소되었을지라도 이러한 변화를 만들어야 한다.