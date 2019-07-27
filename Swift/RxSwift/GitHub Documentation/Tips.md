# 팁

#### Tips

- 항상 시스템과 그 부분들을 순수 함수로 모델링하기 위해 노력하십시오. 그러한 순수 함수는 쉽게 테스트될 수 있고 오퍼레이터의 행위를 수정하기 위해 사용될 수 있습니다.
- Rx를 사용한다면, 내장 오퍼레이터를 먼저 조합하려 하십시오.
- 종종 몇 가지 오퍼레이터의 조합을 사용한다면, 당신의 편의 오퍼레이터를 만드십시오.

예시

```swift
extension ObservableType where E: MaybeCool {

  public func coolElements() -> Observable<E> {
    return filter { e -> Bool in
      return e.isCool
    }
  }
}
```

- Rx 오퍼레이터는 가능한 한 일반적이나, 항상 모델링하기 힘든 엣지 케이스가 있을 것입니다. 이러한 경우 당신만의 오퍼레이터를 만들고 참조로서 내장 오퍼레이터 중 하나를 사용할 수 있을 것입니다.
- 구독을 구성하기 위해 항상 오퍼레이터를 사용하십시오.

**절대로 구독 호출을 중첩하지 마십시오. 코드에 악취를 풍깁니다.**

```swift
textField.rx.text.subscribe(onNext: { text in
  performURLRequest(text).subscribe(onNext: { result in
    ...
  })
  .disposed(by: disposeBag)
})
.disposed(by: disposeBag)
```

**오퍼레이터를 사용하여 disposable들을 체이닝하는 더 나은 방법**

```swift
textField.rx.text
  .flatMapLatest { text in
    // 이것이 실패하지 않고 메인 스케줄러에서 결과를 반환한다고 가정한다.
    // 그렇지 않다면 `catchError`와 `observeOn(MainScheduler.instance)`를 사용하여 이를 고칠 수 있을 것이다.
    return performURLRequest(text)
  }
  ...
  .disposed(by: disposeBag) // 오직 하나의 최상위 disposable
```