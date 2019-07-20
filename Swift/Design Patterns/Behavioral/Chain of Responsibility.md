# Chain of Responsibility

### 책임 연쇄 패턴

> 책임 연쇄 패턴은 다양한 요청을 처리하기 위해 사용되는데, 각 요청은 다른 핸들러에 의해 처리되도록 한다.

명령 객체들과 처리 객체들로 이루어져 있다.

각 처리 객체들은 명령 객체들의 타입을 정의하며, 처리가 되지 않은 것들을 다음 처리 객체에 전달한다.

느슨한 결합을 구현하기 위해 사용되는 좋은 패턴이다.

### 코드

ATM에서 입력한 금액을 인출하는데, 단위가 큰 것부터 돈을 계산하여 준다. (100 -> 50 -> 20 -> 10)

100달러 처리 객체는 50달러 처리 객체를, 50달러 처리 객체는 20달러 처리 객체를, 20달러 처리 객체는 10달러 처리 객체를 갖도록 하여 해당 객체에서 처리하지 못하는 경우 다음 처리 객체에 전달할 수 있도록 한다.

```swift
protocol Withdrawing {
  // `amount` 금액 인출.
  // 금액을 인출할 수 없으면 false를 반환한다.
  func withdraw(amount: Int) -> Bool
}

final class MoneyPile: Withdrawing {
  // 화폐 가치. 100 / 50 / 20 / 10.
  let value: Int
  // 화폐 개수.
  var quantity: Int
  // 다음 처리 객체.
  var next: Withdrawing?
  
  init(value: Int, quantity: Int, next: Withdrawing?) {
    self.value = value
    self.quantity = quantity
    self.next = next
  }
  
  func withdraw(amount: Int) -> Bool {
    var amount = amount
    var quantity = self.quantity
    // 입력으로 들어온 금액을 현재 객체의 화폐 가치로 처리할 수 있는 동안,
    while amount / value > 0 {
      // 남은 화폐가 없는 경우 break.
      if quantity == 0 {
        break
      }
      // 처리.
      amount -= value
      quantity -= 1
    }
    // 처리 종료. 남은 금액에 대한 처리.
    // 남은 금액이 없으면 (현재 객체에서 모든 처리를 완료했으면) true 반환하여 처리 종료.
    guard amount > 0 else {
      return true
    }
    // 남은 금액이 있고 다음 처리 객체가 있으면 다음 처리 객체에 남은 금액에 대한 처리를 넘겨줌.
    if let next = next {
      return next.withdraw(amount: amount)
    }
    // 모든 경우에 맞지 않는 경우 false 반환하여 처리할 수 없음을 나타냄.
    return false
  }
}

final class ATM: Withdrawing {
  private var hundred: Withdrawing
  private var fifty: Withdrawing
  private var twenty: Withdrawing
  private var ten: Withdrawing
  
  private var startPile: Withdrawing {
    return hundred
  }
  
  init(hundred: Withdrawing, fifty: Withdrawing, twenty: Withdrawing, ten: Withdrawing) {
    self.hundred = hundred
    self.fifty = fifty
    self.twenty = twenty
    self.ten = ten
  }
  
  func withdraw(amount: Int) -> Bool {
    // 100 단위부터 처리 로직 실행.
    return startPile.withdraw(amount: amount)
  }
}

/* 사용 */

// 10의 가치를 갖는 화폐를 6개 생성. 다음 처리 객체는 없음.
let ten = MoneyPile(value: 10, quantity: 6, next: nil)
// 20의 가치를 갖는 화폐를 2개 생성. 다음 처리 객체는 ten.
let twenty = MoneyPile(value: 20, quantity: 2, next: ten)
// 50의 가치를 갖는 화폐를 2개 생성. 다음 처리 객체는 twenty.
let fifty = MoneyPile(value: 50, quantity: 2, next: twenty)
// 100의 가치를 갖는 화폐를 1개 생성. 다음 처리 객체는 fifty.
let hundred = MoneyPile(value: 100, quantity: 1, next: fifty)

let atm = ATM(hundred: hundred, fifty: fifty, twenty: twenty, ten: ten)

// 금액이 부족하므로 최종적으로 false를 반환하여 처리하지 못함.
atm.withdraw(amount: 310)
// 100 화폐 가치에서 처리가 완료되어 true를 반환함.
atm.withdraw(amount: 100)
```