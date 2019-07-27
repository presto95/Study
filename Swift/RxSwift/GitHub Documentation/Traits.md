# Traits (Units)

이 문서는 trait이 무엇인지, 왜 그것들이 유용한 개념인지, 어떻게 사용하고 만드는지에 대해 설명하기 위한 것입니다.

## 일반

#### General

### 왜 사용하는가

#### Why

Swift는 애플리케이션의 정확성과 안정성을 향상시키기 위해 사용될 수 있는 강력한 타입 시스템을 가지고 있으며, Rx를 사용하는 것이 더욱 직관적인 경험을 제공할 수 있도록 합니다.

Trait은, 어떠한 컨텍스트에서도 사용될 수 있는 일반적인 Observable과 비교하여, 옵저버블 시퀀스 프로퍼티들이 인터페이스의 경계를 가로질러 상호 작용하고 보장하는 것을 도울 뿐만 아니라, 컨텍스트의 의미와 syntactical sugar를 제공하고, 더욱 특정한 유즈 케이스를 목표로 합니다. **이러한 이유로, Trait은 전적으로 선택적입니다. 모든 코어 RxSwift / RxCocoa API가 그것들을 지원하므로, 프로그램의 어느 부분에서라도 일반적인 Observable 시퀀스를 자유롭게 사용할 수 있습니다.**

***알아두기** : 이 문서에서 설명하는 몇몇 Trait(`Driver` 등)은 `RxCocoa` 프로젝트에만 특정합니다. 다른 것들은 일반적인 RxSwift 프로젝트의 일부분입니다. 그러나 필요하다면 같은 원리는 다른 Rx 구현에서 쉽게 구현될 수 있습니다. 필요한 비공개 접근 수준의 API 마법은 없습니다.*

### 작동하는 방법

#### How they work

Trait은 하나의 읽기 전용 Observable 시퀀스 프로퍼티의 래퍼 구조체일 뿐입니다.

```swift
struct Single<Element> {
  let source: Observable<Element>
}

struct Driver<Element> {
  let source: Observable<Element>
}
```

이것들을 Observable 시퀀스를 위한 어떠한 종류의 빌더 패턴 구현이라고 생각할 수 있습니다. Trait이 만들어질 때, `.asObservable()` 을 호출하여 평범한 옵저버블 시퀀스로 다시 변환될 수 있습니다.

## RxSwift traits

### Single

Single은 Observable의 변형이며, 일련의 요소를 배출하는 대신, 항상 *하나의 요소나 에러*를 배출하는 것을 보장합니다.

- 정확히 하나의 요소, 또는 에러를 배출
- 사이드 이펙트를 공유하지 않음

Single을 사용하는 하나의 흔한 유즈 케이스는 오직 하나의 응답 또는 에러를 반환할 수 있는 HTTP 요청을 수행하는 것에 대한 것입니다. 하지만 Single은 오직 하나의 요소에만 관심이 있고, 요소의 무한한 스트림에 관심이 없는 어떠한 경우에도 모델링하기 위해 사용될 수 있습니다.

#### Single 만들기

Single을 만드는 것은 Observable을 만드는 것과 비슷합니다. 다음의 간단한 예제가 있습니다.

```swift
func getRepo(_ repo: String) -> Single<[String: Any]> {
  return Single<[String: Any]>.create { single in
    let task = URLSession.shared.dataTask(with: URL(string: "https://api.github.com/repos/\(repo)")!) { data, _, error in
      if let error = error {
        single(.error(error))
        return
      }

      guard let data = data,
        let json = try? JSONSerialization.jsonObject(with: data, options: .mutableLeaves),
        let result = json as? [String: Any] else {
        single(.error(DataError.cantParseJSON))
        return
      }

      single(.success(result))
    }

    task.resume()

    return Disposables.create { task.cancel() }
  }
}
```

다음과 같이 이를 사용할 수 있습니다.

```swift
getRepo("ReactiveX/RxSwift")
  .subscribe { event in
    switch event {
    case .success(let json):
      print("JSON: ", json)
    case .error(let error):
      print("Error: ", error)
    }
  }
  .disposed(by: disposeBag)
```

또는 `subscribe(onSuccess:onError:)` 를 사용할 수 있습니다.

```swift
getRepo("ReactiveX/RxSwift")
  .subscribe(onSuccess: { json in
    print("JSON: ", json)
  },
  onError: { error in
    print("Error: ", error)
  })
  .disposed(by: disposeBag)
```

구독은 `SingleEvent` 열거형을 제공하며, 이는 Signle의 타입의 요소를 포함하는 `.success` 나  `.error` 가 될 수 있습니다. 첫 번째의 것 이상으로 배출되는 이벤트는 없습니다.

일반적인 Observable 시퀀스를 Single로 변환하기 위해 `asSingle()` 을 사용할 수도 있습니다.

### Completable

Completable은 오직 *완료되거나 에러를 배출*할 수 있는 Observable의 변형입니다. 이는 어떠한 요소도 배출하지 않는다는 것을 보장합니다.

- 0개의 요소 배출
- 완료 이벤트, 또는 에러를 배출
- 사이드 이펙트를 공유하지 않음

Completable에 대한 유용한 유즈 케이스는 오퍼레이션이 완료되었는지에 대한 사실에만 관심이 있고, 완료에 의한 결과 요소에는 관심이 없는 경우를 모델링하는 것입니다. 요소를 배출할 수 없는 `Observable<Void>` 를 사용하는 것과 비교할 수 있을 것입니다.

#### Completable 만들기

Completable을 만드는 것은 Observable을 만드는 것과 비슷합니다. 간단한 예제가 있습니다.

```swift
func cacheLocally() -> Completable {
  return Completable.create { completable in
    // 로컬에 데이터 저장
    ...
    ...
  }

  guard success else {
    completable(.error(CacheError.failedCaching))
    return Disposables.create { }
  } 
  completable(.completed)
  return Disposables.create { }
}
```

이를 다음과 같이 사용할 수 있습니다.

```swift
cacheLocally() 
  .subscribe { completable in
    switch completable {
    case .completed:
      print("Completed with no error")
    case .error(let error):
      print("Completed with an error: \(error.localizedDescription)")
    }
  }
  .disposed(by: disposeBag)
```

또는 다음처럼 `subscribe(onCompleted:onError)` 를 사용할 수 있습니다.

```swift
cacheLocally()
  .subscribe(onCompleted: {
    print("Completed with no error")
  },
  onError: { error in
    print("Completed with an error: \(error.localizedDescription)")
  })
  .disposed(by: disposeBag)
```

구독은 `CompletableEvent` 열거형을 제공하며, 오퍼레이션이 에러 없이 완료되었다는 것을 알리는 `.completed` 또는 `.error` 가 될 수 있습니다. 첫 번째의 것 이상으로 배출되는 이벤트는 없습니다.

### Maybe

Maybe는 Single과 Completable 사이의 위치에 있는 Observable의 변형입니다. 이는 하나의 요소를 배출하거나, 요소를 배출하는 것 없이 완료되거나, 에러를 배출할 수 있습니다.

**알아두기** : 세 개의 이벤트 중 어느 것이라도 Maybe를 종료시킬 것입니다. 이는 완료된 Maybe는 요소를 배출할 수 없으며, 요소를 배출한 Maybe도 완료 이벤트를 보낼 수 없다는 것을 의미합니다.

- 완료 이벤트 또는 하나의 요소 또는 에러를 배출
- 사이드 이펙트를 공유하지 않음

요소를 배출할 수 있으나 반드시 배출할 필요가 없는 어떠한 오퍼레이션을 모델링하기 위해 Maybe를 사용할 수 있을 것입니다.

#### Maybe 만들기

Maybe를 만드는 것은 Observable을 만드는 것과 비슷합니다. 간단한 예제가 있습니다.

```swift
func generateString() -> Maybe<String> {
  return Maybe<String>.create { maybe in
    maybe(.success("RxSwift")
    
    // 또는

    maybe(.completed)

    // 또는

    maybe(.error(error))

    return Disposables.create { }
  }
}
```

다음과 같이 사용할 수 있습니다.

```swift
generateString()
  .subscribe { maybe in
    switch maybe {
    case .success(let element):
      print("Completed with element \(element)")
    case .completed:
      print("Completed with no element")
    case .error(let error):
      print("Completed with an error \(error.localizedDescription)")
    }
  }
  .disposed(by: disposeBag)
```

또는 `subscribe(onSuccess:onError:onCompleted:)` 를 사용할 수 있습니다.

```swift
generateString()
  .subscribe(onSuccess: { element in
    print("Completed with element \(element)")
  },
  onError: { error in
    print("Completed with an error \(error.localizedDescription)")
  },
  onCompleted: {
    print("Completed with no element")
  })
  .disposed(by: disposeBag)
```

`asMaybe()` 를 일반적인 Observable에 사용하여 그것을 `Maybe` 로 변환할 수 있습니다.

## RxCocoa traits

### Driver

이것은 가장 정교한 trait입니다. 이것의 의도는 UI 레이어에서 리액티브한 코드를 작성하는 직관적인 방법을 제공하는 것이며, 데이터 스트림이 애플리케이션을 *움직이게 하는driving* 것을 모델링하기 원하는 어떠한 경우를 위해서 제공됩니다.

- 에러를 배출할 수 없음
- 메인 스케줄러에서 관찰이 일어남
- 사이드 이펙트를 공유함 (`share(replay: 1, scope: .whileConnected)`)

#### 왜 Driver라고 이름 지어졌는가

이것이 의도하는 유즈 케이스는 애플리케이션을 drive하는 시퀀스를 모델링하는 것입니다.

예를 들어,

- Core Data 모델로부터 UI를 drive한다.
- 다른 UI 요소의 값을 사용하여 UI를 drive한다. (바인딩)

일반적인 운영 체제 드라이버처럼, 시퀀스가 에러를 배출하는 경우 애플리케이션은 사용자 입력에 응답하는 것을 멈출 것입니다.

또한 UI 요소와 애플리케이션 로직은 일반적으로 스레드 안전하지 않기 대문에 그러한 요소가 메인 스레드에서 관찰되는 것은 극도로 중요합니다.

또한, `Driver` 는 사이드 이펙트를 공유하는 관찰 가능한 시퀀스를 만들어 냅니다.

예를 들어,

#### 실제 사용 예제

이는 전형적인 시작 예제입니다.

```swift
let results = query.rx.text
  .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
  .flatMapLatest { query in
    fetchAutoCompleteItems(query)
  }

results
  .map { "\($0.count)" }
  .bind(to: resultCount.rx.text)
  .disposed(by: disposeBag)

results
  .bind(to: resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
    cell.textLabel?.text = "\(result)"
  }
  .disposed(by: disposeBag)
```

이 코드가 의도하는 행위는 다음과 같습니다.

- 사용자 입력을 막는다*throttle*
- 서버와 접촉하여 사용자 결과 리스트를 가져온다 (쿼리 하나하나마다)
- 결과를 두 개의 UI 요소, 결과 테이블 뷰와 결과의 개수를 표시하는 레이블에 바인딩한다.

그래서, 이 코드의 문제점은 무엇입니까?

- 만약 `fetchAutoCompleteItems` 옵저버블 시퀀스가 에러(연결 실패 또는 파싱 에러)를 배출한다면, 이 에러는 모든 것에 바인딩되지 않고 UI는 더 이상 새로운 쿼리에 응답하지 않을 것입니다.
- 만약 `fetchAutoCompleteItems` 가 어떠한 백그라운드 스레드에서 결과를 반환한다면, 결과는 백그라운드 스레드에서 UI 요소에 바인딩될 것이며 이는 비결정적인 충돌의 원인이 될 수 있습니다.
- 결과는 두 개의 UI 요소에 바인딩됩니다. 이는 각각의 사용자 쿼리에 대하여, 각각의 UI 요소 당 하나, 즉 두 개의 HTTP 요청이 만들어지고, 이는 의도한 행위가 아니라는 것을 의미합니다.

더 적절한 버전의 코드입니다.

```swift
let results = query.rx.text
  .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
  .flatMapLatest { query in
    fetchAutoCompleteItems(query)
      .observeOn(MainScheduler.instance)  // 결과는 MainScheduler에서 반환됨
      .catchErrorJustReturn([])           // 최악의 경우 에러가 처리됨
  }
  .share(replay: 1)                       // HTTP 요청은 공유되고 결과는 모든 UI 요소에서 replay됨

results
  .map { "\($0.count)" }
  .bind(to: resultCount.rx.text)
  .disposed(by: disposeBag)

results
  .bind(to: resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
    cell.textLabel?.text = "\(result)"
  }
  .disposed(by: disposeBag)
```

거대한 시스템에서 이러한 모든 요구 사항이 적절하게 처리되게 하는 것은 도전적일 수 있으나, 이러한 요구 사항이 충족되는 것을 증명하기 위해 컴파일러와 trait을 사용하는 더욱 간단한 방법이 있습니다.

```swift
let results = query.rx.text.asDriver()        // 일반적인 시퀀스를 `Driver` 시퀀스로 변환한다.
  .throttle(.milliseconds(300), scheduler: MainScheduler.instance)
  .flatMapLatest { query in
    fetchAutoCompleteItems(query)
      .asDriver(onErrorJustReturn: [])  // 빌더는 에러가 난 경우 반환할 것에 대한 정보만을 필요로 한다.
  }

results
  .map { "\($0.count)" }
  .drive(resultCount.rx.text)               // `bind(to:)` 대신 `drive` 메소드를 사용할 수 있다.
  .disposed(by: disposeBag)              // 이는 컴파일러가 모든 속성이 충족되었음을 증명했다는 것을 의미한다.

results
  .drive(resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
    cell.textLabel?.text = "\(result)"
  }
  .disposed(by: disposeBag)
```

그래서 여기에서 어떤 일이 일어나는가?

첫 번째 `asDriver` 메소드는 `ControlProperty` trait을 `Driver` trait으로 변환한다.

```swift
query.rx.text.asDriver()
```

완료되기 위해 필요한 특별한 것이 없음에 주목하십시오. `Driver` 는 `ControlProperty` trait가 갖는 모든 속성을 가지고 있는 것에 더하여 어떠한 것을 더 가지고 있습니다. 기반 옵저버블 시퀀스는 `Driver` trait로 래핑되었을 뿐이며, 그게 다입니다.

두 번째 변화입니다.

```swift
.asDriver(onErrorJustReturn: [])
```

어떠한 옵저버블 시퀀스라도 `Driver` trait로 변환될 수 있습니다. 다음의 세 가지 속성을 만족하는 한 말입니다.

- 에러를 배출할 수 없음
- 메인 스케줄러에서 관찰
- 사이드 이펙트 공유 (`share(replay: 1, scope: .whiteConnected)`)

어떻게 이러한 프로퍼티를 만족할 수 있게 할 것입니까? 일반적인 Rx 오퍼레이터를 쓰기만 하면 됩니다. `asDriver(onErrorJustReturn: [])` 은 다음의 코드와 동등합니다.

```swift
let safeSequence = xs
  .observeOn(MainScheduler.instance)        // 메인 스케줄러에서 이벤트 관찰
  .catchErrorJustReturn(onErrorJustReturn)  // 에러를 배출할 수 없음
  .share(replay: 1, scope: .whileConnected) // 사이드 이펙트 공유

return Driver(raw: safeSequence)            // 이 모든 것을 래핑함
```

마지막 조각은 `bind(to:)` 대신 `drive` 를 사용하는 것입니다.

`drive` 는 오직 `Driver` trait 에서만 정의됩니다. 이는 코드 어딘가에서 `drive` 를 본다면, 그 옵저버블 시퀀스는 절대 에러를 배출할 수 없고, 메인 스레드에서 관찰하여 UI 요소 바인딩에 안전하다는 것을 의미합니다.

하지만 이론적으로, 누군가가 `ObservableType` 또는 다른 인터페이스에서 동작하는 `drive` 메소드를 여전히 정의할 수 있다는 것에 주목하십시오. 그러니 안전성을 더하기 위해, UI 요소 바인딩 이전에 `let results: Driver<[Results]> = ...` 와 같은 일시적인 정의를 만드는 것은 완전한 증거를 위해 필요할 수 있습니다. 그러나, 우리는 이것이 현실적인 시나리오인지 아닌지는 독자가 판단하도록 남겨둘 것입니다.

### Signal

`Signal` 은 `Driver` 와 비슷한데, 구독에서 최신 이벤트를 리플레이하지 **않는다**는 하나의 차이가 있습니다. 하지만 구독자들은 여전히 시퀀스의 계산된 리소스를 공유합니다.

이것은 애플리케이션의 일부분에서 명령형 이벤트를 리액티브한 방식으로 모델링하는 빌더 패턴으로 고려될 수 있습니다.

`Signal` 은,

- 에러를 배출하지 않음
- 이벤트를 메인 스케줄러에서 전달함
- 계산 리소스를 공유 (`share(scope: .whileConnected)`)
- 구독에서 요소를 리플레이하지 않음

## ControlProperty / ControlEvent

### ControlProperty

UI 요소의 프로퍼티를 나타내는 `Observable` / `ObservableType` 을 위한 trait.

값 시퀀스는 오직 초기 컨트롤 값과 사용자가 일으킨 값 변화를 나타냅니다. 프로그래밍적인 값 변화는 보고되지 않습니다.

이러한 프로퍼티들은,

- 절대 실패하지 않음
- `share(replay: 1)` 행위
  - 상태를 가짐. (subscribe를 호출하여) 구독 중에 마지막 요소는 그것이 생산된다면 즉시 리플레이됨
- 컨트롤이 해제될 때 시퀀스를 `Complete` 함
- 절대 에러를 배출하지 않음
- `MainScheduler.instance` 에서 이벤트를 전달함

`ControlProperty` 의 구현은 이벤트 시퀀스가 메인 스케줄러 (`subscribeOn(ConcurrentMainScheduler.instance)`) 에서 구독되고 있음을 보장할 것입니다.

#### 실제 사용 예제

`UISearchBar+Rx` 와 `UISegmentedControl+Rx` 에서 매우 좋은 실제 예제를 찾을 수 있습니다.

```swift
extension Reactive where Base: UISearchBar {
  /// `text` 프로퍼티에 대한 리액티브 래퍼.
  public var value: ControlProperty<String?> {
    let source: Observable<String?> = Observable.deferred { [weak searchBar = self.base as UISearchBar] () -> Observable<String?> in
      let text = searchBar?.text
            
      return (searchBar?.rx.delegate.methodInvoked(#selector(UISearchBarDelegate.searchBar(_:textDidChange:))) ?? Observable.empty())
        .map { a in
          return a[1] as? String
        }
        .startWith(text)
    }

    let bindingObserver = Binder(self.base) { (searchBar, text: String?) in
      searchBar.text = text
    }
        
    return ControlProperty(values: source, valueSink: bindingObserver)
  }
}
```

```swift
extension Reactive where Base: UISegmentedControl {
  /// `selectedSegmentIndex` 프로퍼티에 대한 리액티브 래퍼.
  public var selectedSegmentIndex: ControlProperty<Int> {
    return value
  }
    
  /// `selectedSegmentIndex` 프로퍼티에 대한 리액티브 래퍼.
  public var value: ControlProperty<Int> {
    return UIControl.rx.value(
      self.base,
      getter: { segmentedControl in
        segmentedControl.selectedSegmentIndex
      }, setter: { segmentedControl, value in
        segmentedControl.selectedSegmentIndex = value
      })
  }
}
```

### ControlEvent

UI 요소에 대한 이벤트를 나타내는 `Observable` / `ObservableType` 을 위한 trait.

이러한 프로퍼티들은,

- 절대 실패하지 않음
- 구독 중에 초기 값을 전달하지 않음
- 컨트롤이 해제될 때 시퀀스를 `Complete` 함
- 절대 에러를 배출하지 않음
- `MainScheduler.instance` 에서 이벤트 전달

`ControlEvent` 의 구현은 이벤트 시퀀스가 메인 스케줄러 (`subscribeOn(ConcurrentMainScheduler.instance)`) 에서 구독된다는 것을 보장할 것입니다.

#### 실제 사용 예제

우리가 사용할 수 있는 전형적인 예제입니다.

```swift
public extension Reactive where Base: UIViewController {
    
  /// Reactive wrapper for `viewDidLoad` message `UIViewController:viewDidLoad:`.
  public var viewDidLoad: ControlEvent<Void> {
    let source = self.methodInvoked(#selector(Base.viewDidLoad)).map { _ in }
    return ControlEvent(events: source)
  }
}
```

그리고 `UICollectionView+Rx` 에서 다음의 방식을 찾을 수 있습니다.

```swift
extension Reactive where Base: UICollectionView {
    
  /// Reactive wrapper for `delegate` message `collectionView:didSelectItemAtIndexPath:`.
  public var itemSelected: ControlEvent<IndexPath> {
    let source = delegate.methodInvoked(#selector(UICollectionViewDelegate.collectionView(_:didSelectItemAt:)))
      .map { a in
        return a[1] as! IndexPath
      }
        
    return ControlEvent(events: source)
  }
}
```