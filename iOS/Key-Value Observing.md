# 시작

야곰의 Swift 4 책의 키패스 부분을 보다가 짜증나서 그냥 넘겼던 기억이 있는데, 이제는 이해할 수 있고, KVO에 대해서도 말만 들어봤지 공부는 안해봤는데 슬슬 해야겠다는 생각이 들었다.

사실 *RespectU* 서버와 그에 따른 클라이언트 코드 수정에 생각보다 너무 많이 애를 먹어서 살짝 추진력이 떨어졌는데, 갑자기 KVO가 생각나서 공부한 것을 정리해둔다.



# KeyPath

먼저 키패스에 대해서 알아보고 가자.

> **키패스**는 어떤 객체의 프로퍼티 혹은 프로퍼티의 프로퍼티 체인에 대해서 그 이름을 통해서 값을 찾아나가는 표현을 말한다.

기본적으로 Objective-C 쪽에서 사용해온 기능인 것 같고, Swift 초기에는 이것에 대한 기능이 별도로 마련되지 않았다고 한다.

문자열을 사용하여 `NSObject` 의 `value(forKey:)` 또는 `setValue(_:forKey:)` 를 사용할 수도 있다.

하지만 문자열을 통해서 키패스를 사용하므로 오타 가능성이 있고, 오타에 의한 존재하지 않는 키패스를 참조하는 것을 확인할 방법이 없어 디버깅이 곤란한 부분이 있다. 또한 이렇게 가져온 데이터는 `Any` 로 오고, `NSObject` 를 상속받아야 기능을 사용할 수 있는 등 여러모로 좋지 않았다.

Swift 4 에서 이러한 단점을 보완하기 위해 모든 Swift 타입에서 키패스를 통해 프로퍼티를 참조할 수 있는 범용적인 키패스 문법과 키패스를 위한 코어 타입이 추가되었다.

```swift
class Struct {
    var a: Int = 3
    var b: Int = 4
}
let object = Struct()
let keyPath = \Struct.a
print(object[keyPath: keyPath])	//3
```

> 모든 Swift 타입에서 키패스를 통해 프로퍼티를 참조할 수 있는 키패스 문법이 추가되었다.

키패스는 `\` 로 시작하고, **타입 이름**을 쓴 후 `.` 를 통해 **프로퍼티에 접근**하면 된다.

타입이 모호하지 않은 경우 바로 `.` 로 시작할 수 있다. (`\Struct.a` / `\.a`)

옵셔널의 경우 `?` 를 붙여서 접근한다.



# Key-Value Observing

키패스를 알아보았으니 키-밸류 옵저빙을 알아볼 때가 되었다.

Swift의 프로퍼티 감시자 (`willSet` , `didSet`) 과 비슷하지만, KVO는 타입이 정의된 곳 바깥에서 옵저버를 추가할 수 있다는 차이점이 있다.

기본적으로 순수한 Swift 로는 작성할 수 없다. Objective-C 런타임에 의존하기 때문에, `NSObject` 를 상속받는 클래스에서 `@objc dynamic` 을 함께 작성하여 프로퍼티를 선언해야 한다.

```swift
class Class: NSObject {
    @objc dynamic var value: String = "안녕"
}
let object = Class()
object.observe(\Class.value) { (object, change) in
    print("changed!!!")
}
print(object.value)
object.value = "Wow"
print(object.value)
//안녕
//Wow
```

이런 식이다. 클로저에 프로퍼티 변경에 따른 적절한 처리를 넘겨줄 수 있다.



# 마무리

모델의 역할을 하는 클래스를 `NSObject` 를 상속받게 하여, 특정 프로퍼티의 변화에 대응할 수 있도록 할 수 있겠다.

*Realm* 을 사용할 때도 유용하게 사용될 수 있을 것 같다.



# 참고한 링크

[Swift4의 키패스 표현](https://soooprmx.com/archives/9177)

[What is key-value observing?](https://www.hackingwithswift.com/example-code/language/what-is-key-value-observing)

[Swift 4 에서 KVO 사용해보기](http://seorenn.blogspot.com/2017/07/swift-4-kvo.html)