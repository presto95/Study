# Delay

### 분류

Observable 유틸리티 연산자

### Observable 연산자 결정 트리

1. Observable이 항목을 배출하기 전에 항목의 배출 시간을 지연시켜야 한다면

---

특정 시간만큼 Observable의 아이템 배출을 지연

---

에러를 배출하는 경우 딜레이되지 않는다.

---

항목을 배출하기 전 딜레이를 주고 싶을 때 사용한다.

```swift
let subject = PublishSubject<Int>()
subject
  .delay(1, scheduler: MainScheduler.instance)
  .subscribe { print($0) }
  .disposed(by: disposeBag)
  
subject.onNext(1)

// next(1)
```

onNext 이벤트가 전달되고 1초 후 콘솔에 구독 결과가 출력된다.