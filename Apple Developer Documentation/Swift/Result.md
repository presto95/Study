# Result

**제네릭 열거형** | 각 케이스에 연관 값을 포함하는, 성공 또는 실패를 나타내는 값

---

## 선언

```swift
enum Result<Success, Failure> where Failure: Error
```

---

`success` 케이스와 `failure` 케이스를 가지고 있으며, 연관 값으로 제네릭 값을 갖는다. `failure` 케이스의 연관 값은 `Error` 프로토콜을 상속받는다.

네트워크 호출 등에서 해당 타입을 사용하면 실제 값 및 에러 처리를 용이하게 할 수 있을 것이다.

### 이니셜라이저

```swift
init(catching body: () throws -> Success)
```

에러를 던질 수 있는 클로저를 평가하여 새로운 `Result` 인스턴스를 만들어 내며, 반환 값을 success케이스의 연관 값으로, 던져진 에러를 failure 케이스의 연관 값으로 만들어낸다.

### 기본 사용 방법

1. switch문을 통하여 success 및 failure 케이스 각각에 대하여 처리할 수 있다.

```swift
let result: Result<Int, Error> = .success(1)
switch result {
  case let .success(value):
    print(value)
  case let .failure(error):
    print(error)
}
```

2. `get()` 메소드를 사용하기. 이 메소드는 에러를 던질 수 있는 메소드로 구현되었다.

```swift
let result: Result<Int, Error> = .success(1)
do {
  // success의 경우
  let value = try result.get()
  print(value)
} catch {
  // failure의 경우
  print(error)
}
```

