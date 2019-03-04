# Merge

**Observable 결합**

여러 개의 Observable의 배출들을 합하여 하나의 Observable로 결합

---

여러 개의 Observable이 각각 아이템을 배출하는 대로 하나의 Observable에 흘려보내어 결합한다.

> 쓸모 없는 코드이지만, Merge가 동작하는 방식을 이해하기 위해 작성한다.

```swift
let subject1 = PublishSubject<Int>()
let subject2 = PublishSubject<Int>()
Observable.merge([subject1, subject2])
  .subscribe { print($0) }
  .disposed(by: disposeBag)

DispatchQueue.global().async {
  (1...5).forEach { subject1.onNext($0) }
}
(101...105).forEach { subject2.onNext($0) }
```

결과는 매번 다르게 나온다. 둘 중 하나의 Subject에 onNext 이벤트가 발생한 순서대로 결합된 Observable에 아이템이 흘러가기 때문이다.