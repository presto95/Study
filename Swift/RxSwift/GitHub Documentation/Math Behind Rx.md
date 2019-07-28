# Rx 뒤에 있는 수학

#### Math Behind Rx

## Observer와 Iterator / Enumerator / Generator / Sequences 사이의 이원성

#### Duality between Observer and Iterator / Enumerator / Generator / Sequences

observer 패턴과 generator 패턴 사이에는 이원성이 있습니다. 이는 비동기 콜백 세계에서 시퀀스 변환의 동기 세계로의 전환을 가능하게 합니다.

말하자면, enuemrator 패턴과 observer 패턴은 모두 시퀀스를 묘사합니다. 왜 enumerator가 시퀀스를 정의하는지는 꽤 명백하지만, observer의 경우 약간 더 복잡합니다.

그러나, 많은 수학적 지식을 요구하지 않는 꽤 간단한 예시가 있습니다. 주어진 시간에 화면에 있는 마우스 커서의 위치를 관찰한다고 가정합시다. 시간이 지나면서 마우스 위치들은 시퀀스를 형성합니다. 이것이 본질적으로 관찰 가능한 시퀀스입니다.

시퀀스의 요소에 접근할 수 있는 두 가지 기본적인 방법이 있습니다.

- 밀어내기 인터페이스*push interface* - Observer (시간이 지나면서 관찰되는 요소는 시퀀스를 만듦)
- 당기기 인터페이스*pull interface* - Iterator / Enumerator / Generator