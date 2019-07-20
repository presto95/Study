# Façade

### 파사드 패턴

> 파사드 패턴은 복합적인 서브시스템에 단순화된 인터페이스를 정의하기 위해 사용된다.

다른 구체 클래스를 직접 가지고 있어 여러 개의 모듈을 하나로 모아 쉽고 단순한 인터페이스를 제공한다.

여러 인터페이스를 통합한 하나의 인터페이스를 제공한다.

### 데코레이터 패턴과의 차이

데코레이터 패턴은 서브클래싱이 아닌 방법을 사용하여 특정 클래스의 기능을 확장한다.

데코레이터 패턴은 런타임에 기능 확장이 필요할 때 사용하며, 파사드 패턴은 컴파일 타임에 기능 확장이 이루어진다.

### 어댑터 패턴과의 차이

어댑터 패턴은 서로 다른 특징을 가진 클래스를 연결한다.

기능을 가진 클래스가 구체 클래스가 아니라면 클래스 어댑터 패턴을 사용한다.

파사드 패턴은 구체 클래스를 직접 가지고 있다.

### 코드

```swift
final class Defaults {
  private let defaults: UserDefaults
  
  init(defaults: UserDefaults = .default) {
    self.defaults = defaults
  }
  
  subscript(key: String) -> String? {
    get {
      return defaults.string(forKey: key)
    }
    set {
      defaults.set(newValue, forKey: key)
    }
  }
}

/* 사용 */

let storage = Defaults()
storage["Bishop"] = "Disconnect me. I'd rather be nothing"
storage["Bishop"]
```