# Factory Method

### 팩토리 메소드 패턴

> 팩토리 메소드 패턴은 인스턴스화된 객체의 타입이 런타임에서 결정될 수 있도록 객체 생성 프로세스를 추상화하여 클래스의 생성자를 대체하기 위해 사용된다.

단순히 객체를 생성하는 메소드를 일컫는 것이 아니다.

상위 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴. 자식 클래스가 어떤 객체를 생성할지 결정하도록 한다.

### 코드

열거형에 객체 생성을 위한 팩토리 메소드를 정의하였다.

```swift
protocol CurrencyDescribing {
  var symbol: String { get }
  var code: String { get }
}

final class Euro: CurrencyDescribing {
  var symbol: String {
    return "€"
  }
  var code: String {
    return "EUR"
  }
}

final class UnitedStatesDollar: CurrencyDescribing {
  var symbol: String {
    return "$"
  }
  var code: String {
    return "USD"
  }
}

enum Country {
  case unitedStates
  case spain
  case uk
  case greece
}

enum CurrencyFactory {
  static func currency(for country: Country) -> CurrencyDescribing? {
    switch country {
    case .spain, .greece:
      return Euro()
    case .unitedStates:
      return UnitedStatesDollar()
    default:
      return nil
    }
  }
}

/* 사용 */

let noCurrencyCode = "No Currency Code Available"

CurrencyFactory.currency(for: .greece)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .spain)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .unitedStates)?.code ?? noCurrencyCode
CurrencyFactory.currency(for: .uk)?.code ?? noCurrencyCode
```