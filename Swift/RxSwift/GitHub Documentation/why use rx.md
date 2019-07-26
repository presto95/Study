# 왜 rx를 사용하는가

**Rx는 선언형으로 앱을 만들 수 있게 해줍니다.**

### 바인딩

```swift
Observable.combineLatest(firstName.rx.text, lastName.rx.text) { $0 + " " + $1 }
  .map { "Greetings, \($0)" }
  .bind(to: greetingLabel.rx.text)
```

`UITableView` 와 `UICollectionView` 에서도 작동합니다.

```swift
viewModel.rows
  .bind(to: resultsTableView.rx.items(cellIdentifier: "WikipediaSearchCell", cellType: WikipediaSearchCell.self)) { _, viewModel, cell in 
  cell.title = viewModel.title
  cell.url = viewModel.url                                                                                                                    }
  .disposed(by: disposeBag)
```

**필수적이지 않더라도, 간단한 바인딩에도 항상 `disposed(by: disposeBag)` 를 사용하는 것을 권장합니다.**

### 재시도

API가 실패하지 않는다면 좋겠지만 그렇지 않을 수도 있습니다. 다음과 같은 API 메소드가 있다고 가정합시다.

```swift
func doSomethingIncredible(forWho: String) throws -> IncredibleThing
```

이 함수를 있는 그대로 사용한다면 실패하는 경우 다시 시도하는 것은 정말로 어렵습니다. 지수 백오프를 모델링하는 복잡함은 말할 것도 없습니다. 사실 재시도하는 것이 가능은 하지만, 신경쓰지 않는 수많은 일시적 상태를 포함하게 될 것이고 재사용 가능하지 않게 될 것입니다.

이상적으로, 당신은 재시도의 본질을 획득하고 어떠한 오퍼레이션에서도 이것을 적용할 수 있기를 원할 것입니다.

Rx를 사용하면 다음과 같이 간단하게 재시도할 수 있습니다.

```swift
doSomethingIncredible("me")
  .retry(3)
```

자신만의 재시도 오퍼레이터도 쉽게 만들 수 있습니다.

### 델리게이트

지루하고 표현적이지 못한 것을 하는 대신,

```swift
public func scrollViewDidScroll(scrollView: UIScrollView) { [weak self]
  self?.leftPositionConstraint.constant = scrollView.contentOffset.x
}
```

다음과 같이 작성할 수 있습니다.

```swift
self.resultsTableView
  .rx.contentOffset
  .map { $0.x }
  .bind(to: self.leftPositionConstraint.rx.constant)
```

### 키-값 옵저빙

```text
`TickTock` was deallocated while key value observers were still registered with it. Observation info was leaked, and may even become mistakenly attached to some other object.

`TickTock`은 키-값 옵저버가 그것에 여전히 등록되어 있는 동안 메모리에서 해제되었다. 관찰 정보는 메모리에서 새고 있으며, 심지어 몇몇 다른 객체에 예기치 않게 부착될 수도 있다.
```

```objective-c
-(void)observeValueForKeyPath:(NSString *)keyPath 
                     ofObject:(id)object 
                       change:(NSDictionary *)change 
                      context:(void *)context
```

위의 것 대신 `rx.observe` 와 `rx.observeWeakly` 를 사용하십시오.

```swift
view.rx.observe(CGRect.self, "frame")
  .subscribe(onNext: { frame in 
    print("Got new frame \(frame)")
  })
  .disposed(by: disposeBag)
```

```swift
someSuspiciousViewController
  .rx.observeWeakly(Bool.self, "behavingOk")
  .subscribe(onNext: { behavingOk in
    print("Cats can purr? \(behavingOk)")
  })
  .disposed(by: disposeBag)
```

위의 코드처럼 사용할 수 있습니다.

### 노티피케이션

```swift
@available(iOS 4.0, *)
public func addObserver(forName name: NSNotification.Name?, object obj: Any?, queue: OperationQueue?, using block: @escaping (Notification) -> Void) -> NSObjectProtocol
```

위의 것을 사용하는 대신,

```swift
NotificationCenter.default
  .rx.notification(NSNotification.Name.UITextViewTextDidBeginEditing, object: myTextView)
  .map { /* do something with data */ }
  ....
```

위처럼 작성할 수 있습니다.

### 일시적 상태

#### Transient state

비동기 프로그램을 작성할 때 일시적 상태를 사용하는 것으로 발생할 수 있는 많은 문제들이 있습니다. 전형적인 예제는 자동완성되는 검색 상자입니다.

Rx 없이 자동완성 코드를 작성한다면, 아마 `abc` 에 있는 `c` 가 타이핑되었을 때, 그리고 `ab` 에 대한 완료되지 않은 요청이 있을 때, 그 요청을 취소해야 하는 것을 해결해야 할 필요가 있다는 것이 첫 번째 문제로 다가올 것입니다. 물론 이는 해결하기에 그렇게 어렵지는 않습니다. 완료되지 않은 요청에 대한 참조를 갖고 있는 변수를 하나 더 생성해주기만 하면 됩니다.

다음 문제는 요청이 실패했을 때 발생합니다. 지저분한 재시도 로직을 사용하여 재시도를 할 필요가 있습니다. 하지만 괜찮습니다. 정리될 필요가 있는, 재시도 횟수를 파악할 수 있는 더 많은 필드를 가질 수 있습니다.

프로그램이 서버에 요청하기 전 조금 기다린다면 좋을 것입니다. 말하자면, 누군가가 매우 긴 무언가를 타이핑하는 프로세스에 있는 경우 우리 서버에 너무 많은 요청을 하는 것을 원하지 않습니다. 아마 추가적인 타이머 필드를 필요로 할 것입니다.

검색 작업이 수행 중일 때, 모든 재시도 끝에도 실패한 경우에 어떤 것이 화면에 보여질 필요가 있는지에 대한 질문도 있습니다.

이 모든 것을 작성하고 적절하게 테스트하는 것은 지루한 작업일 것이빈다. Rx를 사용하여 같은 로직을 다음과 같이 작성할 수 있습니다.

```swift
searchTextField.rx.text
  .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
  .distinctUntilChanged()
  .flatMapLatest { query in 
    API.getSearchResults(query)
      .retry(3)
      .startWith([])  // 새로운 검색 조건에 대하여 결과 지우기
      .catchErrorJustReturn([])
  }
  .subscribe(onNext: { results in 
    // UI 바인딩
  })
  .disposed(by: disposeBag)
```

추가적인 플래그나 필드를 요구하지 않습니다. Rx가 모든 일시적인 것들을 관리합니다.

### 조합 처분

#### Compositional disposal

테이블 뷰에 블러 효과가 있는 이미지를 표시하기를 원하는 시나리오가 있다고 가정합시다. 먼저 이미지는 URL로부터 가져와질 것이고 디코딩된 후 블러 처리될 것입니다.

대역폭과 블러 효과를 주기 위한 처리 시간은 비용이 많이 들기 때문에 셀이 테이블 뷰의 화면에 보이는 영역을 이탈했다면 전체 프로세스가 취소될 수 있게 하는 것이 좋을 것입니다.

또한 셀이 화면에 보이는 영역에 들어갔을 때 이미지를 가져오는 작업을 즉시 수행하지 않게 하는 것도 좋을 것이빈다. 사용자는 매우 빠르게 스와이프하고, 이 경우 많은 요청이 수행되고 취소될 것입니다.

병렬 이미지 작업의 숫자에 제한을 거는 것도 좋을 것입니다. 이미지에 블러 효과를 주는 작업은 비용이 많이 드는 것이기 때문입니다.

Rx를 사용하여 다음과 같이 작성하여 작업을 수행할 수 있습니다.

```swift
// 개념적 해결법입니다.
let imageSubscription = imageURLs
  .throttle(.milliseconds(200), scheduler: MainScheduler.instance)
  .flatMapLatest { imageURL in 
    API.fetchImage(imageURL)
  }
  .observeOn(operationScheduler)
  .map { imageData in decodeAndBlurImage(imageData) }
  .observeOn(MainScheduler.instance)
  .subscribe(onNext: { blurredImage in
    imageView.image = blurredImage
  })
  .disposed(by: reuseDisposeBag)
```

이 코드가 모든 것을 수행할 것이고, `imageSubscription` 이 dispose되었을 때 모든 의존적인 비동기 오퍼레이션을 취소할 것이고, 화면에 보여지는 영역을 벗어난 셀과 연관된 이미지가 UI에 바인딩되는 일이 없게 할 것입니다.

### 네트워크 요청 결합

#### Aggregating network requests

두 개의 요청을 보내고 두 개 모두 완료되었을 때 결과들을 결합하기 원한다면 어떻게 하겠습니까?

물론 `zip` 오퍼레이터가 있습니다.

```swift
let userRequest: Observable<User> = API.getUser("me")
let friendsRequest: Observable<[Friend]> = API.getFriends("me")

Observable.zip(userRequest, friendsRequest) { user, friends in 
  return (user, friends)
}
.subscribe(onNext: { user, friends in 
  // UI 바인딩
})
.disposed(by: disposeBag)
```

API가 백그라운드 스레드에서 결과를 반환하고, 메인 UI 스레드에서 바인딩 작업이 발생한다면 어떻게 하겠습니까?

`observeOn` 을 사용할 수 있습니다.

```swift
let userRequest: Observable<User> = API.getUser("me")
let friendsRequest: Observable<[Friend]> = API.getFriends("me")

Observable.zip(userRequest, friendsRequest) { user, friends in 
  return (user, friends)
}
.observeOn(MainScheduler.instance)
.subscribe(onNext: { user, friends in 
  // UI 바인딩
})
.disposed(by: disposeBag)
```

Rx가 정말로 빛나는 실용적인 유즈 케이스가 매우 많이 있습니다.

### 상태

#### State

변경을 허용하는 언어는 전역 상태에 접근하고 그것을 변경하기 쉽게 합니다. 공유되는 전역 상태에 대한 통제되지 않는 변경은 조합 폭발*combinatorial explosion*을 쉽게 불러 일으킬 수 있습니다.

> combinatorial explosion : 문제의 조합론이 문제의 입력, 제약 및 경계에 의해 어떻게 영향을 받는지에 따라 문제의 복잡성이 급격하게 증가하는 것

반면 이를 영리하게 활용하면, 명령형 언어는 하드웨어에 가까운 더욱 효율적인 코드를 작성할 수 있게 해줍니다.

조합 폭발과 싸우는 일반적인 방법은 상태를 가능한 한 간단하게 유지하고, 파생된 데이터를 모델링하기 위해 단방향 데이터 흐름을 사용하는 것입니다.

이것이 Rx가 정말로 빛나는 부분입니다.

Rx는 함수형 및 명령형 세계 사이의 스윗 스팟에 있습니다. 이는 신뢰할 수 있고 조합 가능한 방식으로 변경 가능한 상태의 스냅샷을 처리하기 위해 변하지 않는 정의와 순수 함수를 사용할 수 있게 합니다.

실제적인 예는 무엇이 있을까요?

### 쉬운 통합

#### Easy integration

당신만의 observable을 만들어야 한다면 어떻게 하시겠습니까? 이는 꽤 쉽습니다. 이 코드는 RxCocoa로부터 취해지고, `URLSession` 을 사용하여 HTTP 요청을 감싸는 것이 당신이 해야 하는 전부입니다.

```swift
extension Reactive where Base: URLSession {
  public func response(request: URLRequest) -> Observable<(Data, HTTPURLResponse)> {
    return Observable.create { observer in
      let task = self.base.dataTask(with: request) { data, response, error in
        guard let response = response, let data = data else {
          observer.on(.error(error ?? RxCocoaURLError.unknown))
          return
        }
        guard let httpResponse = response as? HTTPURLResponse else {
          observer.on(.error(RxCocoaURLError.nonHTTPResponse(response: response)))
          return
        }
        observer.on(.next(data, httpResponse))
        observer.on(.completed)
      }
      task.resume()
      return Disposables.create(with: task.cancel)
    }
  }
}
```

### 이득

#### Benefits

말하자면, Rx를 사용하면 당신의 코드를 이렇게 만들 수 있습니다.

- 조합 가능하게 : Rx는 구성의 닉네임
- 재사용 가능하게 : 조합 가능함
- 선언적으로 : 정의는 불변하고 데이터만 변화함
- 이해하기 쉽고 명료하게 : 추상화 수준을 높이고 일시적 상태를 제거함
- 안정적으로 : Rx 코드는 철저하게 단위 테스트됨
- 상태에 덜 의존하게 : 단방향 데이터 흐름을 갖도록 애플리케이션을 모델링함
- 메모리 누수 없이 : 리소스 관리가 쉬움

### 이게 다가 아닙니다.

#### It's not all or nothing

Rx를 사용하여 애플리케이션을 가능한 한 많이 모델링하는 것은 좋은 생각입니다.

하지만 모든 오퍼레이터에 대해 알지 못하고 특정 케이스를 모델링하는 어떠한 오퍼레이터가 존재하지 않는다면 어떻게 하시겠습니까?

모든 Rx 오퍼레이터는 수학에 기반하며 직관적일 것입니다.

좋은 소식은 약 10~15개의 오퍼레이터가 대부분의 전형적인 유즈 케이스를 커버한다는 것입니다. 그리고 그 목록은 이미 익숙한 `map`, `filter`, `zip`, `observeOn` 과 같은 것을 포함합니다.

[모든 Rx 오퍼레이터](http://reactivex.io/documentation/operators.html)에 대한 리스트를 참고하십시오.

각각의 오퍼레이터에 대해 그것이 동작하는 방법을 설명하는 것을 돕는 마블 다이어그램이 있습니다.

하지만 리스트에 없는 오퍼레이터를 필요로 한다면? 당신은 자신만의 오퍼레이터를 만들 수 있습니다.

그러한 종류의 오퍼레이터를 만드는 것은 어떠한 이유에서 어렵다든가, 상태를 갖는 어떠한 레거시 코드를 사용하여 작업해야 할 필요가 있다면 어떻게 하시겠습니까? 스스로 혼란스러워질 수 있습니다. 하지만 [Rx의 모나드](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#life-happens) 개념을 쉽게 뛰어넘고, 데이터를 처리하고, 그것으로 다시 돌아갈 수 있습니다.