# 9장 RxSwift 응용해보기

## RxCocoa

- `Cocoa Touch Framework` 의 Reactive Extension

---

- `asObservable()` 을 사용하여 명시적으로 `Observable` 타입을 만들어주면 코드 제안 기능을 사용할 수 있음

---

Input과 Output을 구분하여 생각해보기

```swift
// input : 아이디 및 비밀번호 입력
let idInputObservable = idField.rx.text.orEmpty.asObservable()
let passwordInputObseravble = pwField.rx.text.orEmpty.asObservable()
let idValidObservable = idInputObservable.map(checkEmailValid)
let passwordValidObservable = passwordInputObseravble.map(checkPasswordValid)
        
// output : validView isHidden 및 button isEnabled
idValidObservable
	.subscribe(onNext: { isValid in
		self.idValidView.isHidden = isValid
	})
	.disposed(by: disposeBag)
passwordValidObservable
	.subscribe(onNext: { isValid in
		self.pwValidView.isHidden = isValid
	})
	.disposed(by: disposeBag)
Observable
	.combineLatest(idValidObservable, passwordValidObservable) { $0 && $1 }
	.subscribe(onNext: { isValid in
		self.loginButton.isEnabled = isValid
	})
	.disposed(by: disposeBag)
```

위와 같이 이벤트를 등록하여 비동기적으로 수행할 작업을 등록해줄 수 있음