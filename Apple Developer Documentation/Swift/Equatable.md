# Equatable

**프로토콜** | 값의 동등함 비교가 가능한 타입

---

### 개요

`Equatable` 프로토콜을 준수하는 타입은 `==` 연산자와 `!=` 연산자를 사용하여 동등함과 동등하지 않음을 비교할 수 있다.

Swift 스탠다드 라이브러리에 정의된 대부분의 기본 타입은 `Equatable` 프로토콜을 준수한다.

몇몇 시퀀스 및 컬렉션 작업은 그 요소가 `Equatable` 프로토콜을 준수할 때 더욱 단순하게 사용될 수 있다.

- `contains(_:)` 메소드에 값을 넘겨 시퀀스 작업을 더욱 쉽게 할 수 있음

### Equatable 프로토콜 준수

커스텀 타입이 `Equatable` 프로토콜을 준수하는 것은 컬렉션 내의 특정 인스턴스를 검색하는 것에 있어서 편리한 API를 사용할 수 있다는 것을 의미한다.`

`Equatable` 프로토콜은 `Hashable` 및 `Comparable` 프로토콜의 기반 프로토콜이다.

구조체가 `Equatable` 프로토콜을 준수하는 경우, 그 모든 저장 프로퍼티는 `Equatable` 프로토콜을 준수해야 한다.

열거형이 `Equatable` 프로토콜을 준수하는 경우, 그 모든 연관 값은 `Equatable` 프로토콜을 준수해야 한다. 연관 값이 없는 열거형은 암시적으로 `Equatable` 프로토콜을 준수한다.

프로토콜을 준수하기 위해 == 연산자를 구현해야 한다. 그러면 스탠다드 라이브러리가 != 연산자를 구현해 준다.

---

동등함은 대체 가능함을 암시한다. 동등하게 비교되는 어떠한 두 개의 인스턴스는 그 값에 따라 달라지는 코드 어떠한 곳에서도 상호 변경되도록 사용될 수 있다. 대체 가능함을 유지하기 위해 == 연산자는 `Equatable` 타입의 모든 가시적인 양상을 고려해야 한다. 클래스의 동일함 이외에 값이 아닌 양상을 드러내는 것은 바람직하지 않으며, 드러내진 것은 문서에서 명시적으로 지적되어야 한다.

a == a 는 항상 true / a == b 는 b == a 을 암시함 / a == b 와 b == c 는 a == c 를 암시함이 명백해야 함.

#### 동등함과 동일함은 분리된다. *Equality is Separate From Identity.*

클래스 인스턴스의 동일함은 인스턴스의 값과 관련한 부분이 아니다. 

클래스 인스턴스의 동일함 비교를 위해 === 연산자를 사용할 수 있다.

---

