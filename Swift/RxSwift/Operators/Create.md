# Create

**Observable 생성**

함수를 사용하여 Observable을 처음부터 생성

---

프로그래머가 원하는 동작을 하는 Observable을 만들기 위해 사용한다.

```swift
Observable<Int>.create { observer in
  let alert = UIAlertController(title: nil, message: nil, preferredStyle: .alert)
  let action = UIAlertAction(title: "", style: .default) { _ in
    observer.onNext(1)
    observer.onCompleted()
  }
  alert.addAction(action)
  self.present(alert, animated: true, completion: nil)
  return Disposables.create {
    alert.dismiss(animated: true, completion: nil)
  }
}
.subscribe { print($0) }
.disposed(by: disposeBag)

// 얼러트를 띄우고 버튼을 누르면
// next(1)과 completed가 차례대로 콘솔에 출력된다.
```

