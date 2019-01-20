# 10장 Subject

## Subject

- 데이터가 나중에 발생했을 때 외부에서 추가할 수 있는 `Observable`

### AsyncSubject

- 기존 스트림이 `Complete` 되는 시점에 가장 마지막에 전달된 데이터를 전달함

### BehaviorSubject

- 초기값을 받음, 이후 스트림에 존재했던 데이터를 받음
- 어떠한 데이터 이후에 `subscribe` 하는 경우 해당 시점의 마지막 데이터를 받은 후 이후 데이터를 받음

### PublishSubject

- 초기값이 없음. 나중에 데이터가 발생하면 전달함

### ReplaySubject

- `subscribe` 할 때 기존에 존재했던 데이터를 모두 전달함

```swift
let idValid = BehaviorSubject(value: false)
let idInputObservable = idField.rx.text.orEmpty
// idValid에 idInputObservable을 흘려보냄. 두 가지 방법
// Observable에 Subject를 바인딩
idInputObservable.bind(to: idValid)
// Observable의 onNext 이벤트에서 온 데이터를 Subject에 흘려보냄
idInputObservable.subscribe(onNext: { self.idValid.onNext($0) }
```

---

`Subject` 를 프로퍼티로 정의하여 바깥에 두고, 이것에 접근하는 방식으로 코드를 작성할 수 있음

`bind` 나 `onNext` 를 사용하여 값을 `Subject` 에 묶어야 함