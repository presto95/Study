# Mediator

### 중재자 패턴

> 중재자 패턴은 서로 상호 작용하는 클래스 간 결합도를 줄이기 위해 사용된다. 직접적으로 상호 작용하는 클래스 대신, 그들의 구현에 대한 지식이 필요하므로 중재자 객체를 통해 메세지를 전달한다.

객체들의 집합이 상호 작용하는 방식을 함축한 객체를 정의하며, 이것이 중재자 객체가 된다.

객체 간 의존성을 줄여 결합도를 감소시킨다.

런타임 시의 행위를 중재자 객체에 묶어둔다.

프로그램에 존재하는 수많은 클래스 간 상호 작용을 분리하여 유지 보수성을 높인다.

### 코드

수신자를 추상화한 Receiver 프로토콜, 전송자를 추상화한 Sender 프로토콜을 정의한다.

Sender를 채택하는 중재자 클래스를 정의하고, 해당 객체를 사용하여 클래스 간 상호 작용을 구현한다.

```swift
protocol Receiver {
  associatedType MessageType
  func receive(message: MessageType)
}

protocol Sender {
  associatedType MessageType
  associatedType ReceiverType: Receiver
  var recipients: [ReceiverType] { get }
  func send(message: MessageType)
}

struct Programmer: Receiver {
  let name: String
  
  func receive(message: String) {
    print("\(name) received: \(message)")
  }
}

final class MessageMediator: Sender {
  var recipients: [Programmer] = []
  
  func add(recipient: Programmer) {
    recipients.append(recipient)
  }
  
  func send(message: String) {
    for recipient in recipients {
      recipient.receive(message: message)
    }
  }
}

/* 사용 */

// 중재자 객체를 참조하여 객체 간 상호 작용을 실현한다.
func spamMonster(message: String, worker: MessageMediator) {
  worker.send(message: message)
}

let messagesMediator = MessageMediator()

let user0 = Programmer(name: "Linus Torvalds")
let user1 = Programmer(name: "Avadis 'Avie' Tevanian")
messagesMediator.add(recipient: user0)
messagesMediator.add(recipient: user1)

spamMonster(message: "I'd Like to Add you to My Professional Network", worker: messagesMediator)
```