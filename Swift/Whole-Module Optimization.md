# Whole-Module Optimization

- Swift 컴파일러의 최적화 모드
- 프로젝트에 따라 취할 수 있는 이득의 정도가 달라짐
  - 2~5배 정도
- `-whole-module-optimization` 또는 `-wmo` 컴파일러 플래그로 활성화
  - Xcode 8부터 기본적으로 설정됨
- Swift Package Manager는 릴리즈 빌드에 대하여 WMO를 사용하여 컴파일함

## 모듈 / 컴파일 방법

- 모듈*module* : Swift 파일의 집합
  - 각 모듈은 프레임워크 또는 실행 파일(단일 배포 단위)로 컴파일됨
- WMO가 아닌 단일 파일 컴파일*single-file compilation*에서 Swift 컴파일러는 모듈에 있는 각각의 파일을 개별적으로 컴파일함
  - 컴파일러 드라이버나 Xcode 빌드 시스템에 의해 일어남

```text
                /--------\
file_1.swift -> | swiftc | -> file_1.o --\
                \--------/                \
                /--------\                 \  /----\
file_2.swift -> | swiftc | -> file.2_o -----> | ld | -> binary
                \--------/                 /  \----/
                /--------\                /
file_n.swift -> | swiftc | -> file.n_o --/
                \--------/
```

컴파일 순서

1. 소스 파일을 읽고 파싱하며, 타입 검사와 같은 몇 가지 작업을 더 수행함
2. Swift 코드 최적화
3. 기계어 생성
4. 오브젝트 파일 작성
5. 모든 오브젝트 파일 링크
6. 공유 라이브러리 또는 실행 파일 생성

---

- 단일 파일 컴파일에서, 컴파일러 최적화의 범위는 단일 파일임
  - 함수 인라이닝*function inlining*, 제네릭 특수화*generic specialization*과 같은 함수 교차 최적화*cross-function optimization*가 같은 파일에서 호출되고 정의된 함수들로 제한됨

---

예제

- 모듈은 utils.swift 파일만을 포함함
  - `Container<T>` 제네릭 구조체 포함
  - `getElement` 메소드 포함
  - 위의 메소드는 모듈 곳곳에서 호출됨

```swift
// main.swift
func add(c1: Container<Int>, c2: Container<Int>) -> Int {
  return c1.getElement() + c2.getElement()
}

// utils.swift
struct Container<T> {
  var element: T
  
  func getElement() -> T {
    return element
  }
}
```

- 컴파일러는 main.swift를 최적화할 때 `getElement`가 구현된 방법에 대해 알지 못함
  - 그것이 존재한다는 것만을 앎
  - 그러므로 `getElement`에 대한 호출을 생성함
- 컴파일러는 utils.swift를 최적화할 때 함수가 호출되는 구체 타입에 대해 알지 못함
  - 함수의 제네릭 버전만을 생성할 수 있음
    - 구체 타입에 대해 특수화된 것보다 훨씬 느림
  - `getElement`의 단순한 구현(`return element`)은 요소를 복사하는 방법을 알기 위해 타입의 메타데이터를 찾아야 할 필요가 있음
    - `Int`와 같은 단순한 타입일 수도 있고, 더 큰 크기의 타입일 수도 있고, 참조 카운팅 작업을 하는 타입을 포함한 것일 수도 있음
    - 컴파일러는 위와 같은 정보를 알지 못함

## 전체 모듈 최적화

WMO를 통해 컴파일러는 모듈에 있는 모든 파일을 전체적으로 최적화함

```text
file_1.swift --\   /--------\
file_2.swift ----> | swiftc | -> output files
file_n.swift --/   \--------/
```

- **컴파일러는 모듈에 있는 모든 함수의 구현을 앎**
  - 함수 인라이닝, 함수 특수화와 같은 최적화를 수행할 수 있음
  - 함수 특수화 : 컴파일러가 특정 호출 컨텍스트에 대해 최적화된 새로운 버전의 함수를 생성
    - 제네릭 함수를 구체 타입에 대해 특수화할 수 있음

---

- 예제에서 컴파일러는 `Int` 구체 타입에 대해 특수화된 제네릭 `Container`의 한 버전을 생성할 수 있음

```swift
// `Int` 타입으로 구체화된 `Container` 제네릭 구조체
struct Container {
  var element: Int
  
  func getElement() -> Int {
    return element
  }
}
```

- 그 결과 컴파일러는 특수화된 `getElement` 함수를 `add` 함수로 인라이닝할 수 있음

```swift
func add(c1: Container<Int>, c2: Container<Int>) -> Int {
  // 이전 : c1.getElement() + c2.getElement()
  return c1.element + c2.element
}
```

- 단일 파일 최적화에서 `add` 함수는 두 개의 함수 호출을 포함하나, 전체 모듈 최적화에서 `add` 함수는 함수 호출을 하지 않음

---

- 컴파일러가 함수를 인라이닝하지 않는다고 결정할지라도, 컴파일러가 함수의 구현을 알고 있다면 많은 도움이 됨
  - 참조 카운팅 작업과 관련된 행위를 추론할 수 있음
  - 이를 바탕으로 함수 호출 주위에 있는 중복된 참조 카운팅 작업을 제거할 수 있음
- **공개 수준이 아닌 함수들의 모든 사용에 대해 추론할 수 있음**
  - 공개 수준이 아닌 함수들은 모듈 내에서만 사용 가능하므로, 컴파일러는 이러한 함수들의 모든 참조들을 알고 있다고 확신할 수 있음
  - 이를 바탕으로 '죽은' 함수와 메소드를 제거할 수 있음
    - '죽은' 함수 : 절대 호출되지 않거나 사용되지 않는 함수 및 메소드
    - 프로그래머가 작성하였으나 전혀 사용되지 않는 코드의 경우에도 컴파일러가 제거할 수 있음
    - 종종 함수들은 다른 최적화의 부작용으로 죽게 되며, 이러한 경우 컴파일러가 제거할 수 있는 것이 좋은 유스케이스임
      - `add` 함수가 `Container.getElement`가 호출되는 유일한 장소라고 가정했을 때
        - `getElement`를 인라이닝한 후 해당 함수는 더이상 사용되지 않으므로 제거할 수 있음
        - `getElement`를 인라이닝하지 않기로 결정할지라도, `add` 함수는 `getElement`의 특수화된 버전만을 호출하므로 `getElement`의 기존 제네릭 버전을 제거할 수 있음

## 컴파일 타임

- 단일 파일 컴파일의 경우 컴파일러 드라이버는 개별 프로세스에서 각각의 파일에 대해 컴파일함
  - 병렬적으로 수행될 수 있음
- 마지막 컴파일 이후 수정되지 않은 파일들은 다시 컴파일될 필요가 없음
  - 증분 컴파일
  - 컴파일 타임을 많이 절약할 수 있음

```text
                   /------------ swiftc -------------\
file_1.swift --\   |                     /-- LLVM  --|-- file_1.o --\    /----\
file_2.swift ------|--> SIL optimizer ------ LLVM  --|-- file_2.o -----> | ld | -> binary
file_n.swift --/   |                     \-- LLVM  --|-- file_n.o --/    \----/
                   \---------------------------------/
```

- 내부적으로 컴파일러는 여러 개의 단계(파서, 타입 검사, SIL 최적화, LLVM 백엔드)에서 동작함
  - 파싱과 타입 검사는 대부분의 경우 매우 빠르게 일어남. Swift의 이후 릴리즈에서 더욱 빨라질 것이 기대됨
  - SIL 최적화기*SIL optimizer*는 Swift에 특정한 중요한 최적화(제네릭 특수화, 함수 인라이닝 등)를 수행함
    - 전체 컴파일 타임의 3분의 1 정도 소요
  - LLVM 백엔드는 컴파일 타임의 대부분을 소비함
    - 저수준 최적화 수행, 코드 생성
  - SIL 최적화기에서 전체 모듈 최적화 수행 후, 모듈은 다시 여러 개의 부분으로 나뉘어짐
    - LLVM 백엔드는 여러 개의 쓰레드에서 나뉘어진 부분을 처리함	
    - 이전 빌드 이후 변경되지 않은 부분이 있다면 해당 부분을 다시 처리하지 않음
    - 그러므로 컴파일러는 컴파일 작업의 큰 부분을 병렬적으로(멀티쓰레드), 증분으로 수행할 수 있음

## 결론

- 전체 모듈 최적화는 Swift 코드를 모듈 내의 파일 간에 분배하는 방법에 대해 걱정할 필요 없이 최대의 퍼포먼스를 낼 수 있도록 하는 훌륭한 방법임
- 최적화가 임계 코드 구역*critical code section*에서 일어난다면, 퍼포먼스는 단일 파일 최적화와 비교하여 다섯 배까지 더 나아질 수 있음
- 컴파일 타임 또한 더 나아질 수 있음

## 참고

[Whole-Module Optimization in Swift 3](https://swift.org/blog/whole-module-optimizations/)