# State

### 상태 패턴

> 상태 패턴은 내부 상태가 변경될 때 객체의 행위를 변경하기 위해 사용된다. 이 패턴은 객체의 클래스가 런타임에 변경될 수 있도록 한다.

정보 객체를 상태 객체로 넘겨 상태 객체마다 다른 정보를 담는다.

객체 지향 방식으로 상태 기계를 구현한다.

상태 패턴 인터페이스의 파생 클래스로 각각의 상태를 구현한다.

런타임에서 객체의 행위를 손쉽게 바꿀 수 있다.

### 코드

컨텍스트 객체를 상태 객체로 넘겨 해당 상태를 가져온다.

```swift
// 컨텍스트 객체가 상태를 담는다.
final class Context {
  private var state: State = Unauthorized()
  
  var isAuthorized: Bool {
    return state.isAuthorized(context: self)
  }
  
  func changeStateToAuthorized(userID: String) {
    state = AuthorizedState(userID: userID)
  }
  
  func changeStateToUnauthorized() {
    state = UnauthorizedState()
  }
}

// 상태 인터페이스 정의.
protocol State {
  func isAuthorized(context: Context) -> Bool
  func userID(context: Context) -> String?
}

// 상태 인터페이스를 구현하는 각각의 상태 정의.
class UnauthorizedState: State {
  func isAuthorized(context: Context) -> Bool {
    return false
  }
  
  func userID(context: Context) -> String? {
    return nil
  }
}

// 상태 인터페이스를 구현하는 각각의 상태 정의.
class AuthorizedState: State {
  let userID: String
  
  init(userID: String) {
    self.userID = userID
  }
  
  func isAuthorized(context: Context) -> Bool {
    return true
  }
  
  func userID(context: Context) -> String? {
    return userID
  }
}

/* 사용 */

let userContext = Context()
(userContext.isAuthorized, userContext.userID)
userContext.changeStateToAuthorized(userID: "admin")
(userContext.isAuthorized, userContext.userID)
userContext.changeStateToUnauthorized()
(userContext.isAuthorized, userContext.userID)
```