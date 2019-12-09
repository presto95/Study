# iOS의 레이아웃 사이클

## UIView와 UIViewController의 레이아웃 사이클

`UIViewController`는 기본적으로 루트 뷰(`UIView`)를 가지고 있다.

`UIViewController`를 instantiate했을 때, 시스템은 다음의 메소드들을 순차적으로 호출한다.

### 시작

1. **requiresConstraintBasedLayout**
   - `UIView`의 타입 프로퍼티
   - 리시버가 제약 기반 레이아웃 시스템에 의존하는지를 나타내는 불리언 값을 나타낸다.
2. **loadView**
   - `UIViewController`의 메소드
   - 컨트롤러가 관리하는 뷰를 생성한다.
1. **viewDidLoad**
   - `UIViewController`의 메소드
   - 컨트롤러의 뷰가 메모리에 로드된 후 호출된다.
2. **viewWillAppear**
   - `UIViewControlelr`의 메소드
   - 뷰 컨트롤러에게 뷰가 뷰 계층에 추가되려 함을 알린다.

### Constraints 관련

오토레이아웃의 제약을 갱신한다. subview에서 superview 순서대로 갱신된다.

1. **updateConstraints**
  
   - `UIView`의 메소드
   - 뷰의 제약 사항을 갱신한다.
   - 제약 변경을 최적화하기 위해 이 메소드를 재정의할 수 있다. 이 때 `setNeedsLayout`, `layoutIfNeeded`, `setNeedsDisplay`와 같은 메소드를 호출하면 안된다. 레이아웃이 결정된 시점이 아니기 때문이다.
   - 메소드를 재정의할 때 super 메소드는 구현부 가장 마지막에 작성한다.
   - 시스템이 적절한 타이밍에 호출해준다. 직접 호출해서는 안 된다.
     - 제약 변경을 스케줄링하기 위해 `setNeedsUpdateConstraints` 메소드를 호출할 수 있다. 이 메소드는 호출되는 순간 제약을 갱신하지 않고, 시스템이 한번에 정리하여 제약을 갱신한다.
     - 시스템이 제약의 변경을 실행할 때 `updateConstraintsIfNeeded`를 호출한다. 이 메소드는 직접 호출할 수 있다.
2. **intrinsicContentSize**
  
   - `UIView`의 프로퍼티
   
   - 뷰의 고유 크기를 나타내며, 뷰 자체의 프로퍼티만 고려한다.
   
   - 뷰는 Intrinsic Content Size(고유 컨텐츠 크기)를 가지고 있다.
     - `UILabel`의 경우 텍스트를 알맞게 감쌀 수 있는 크기(horizontal 및 vertical 모두)가 고유 크기다.
     - `UIProgressView`의 경우 최소 높이(vertical만)가 고유 크기다.
     
   - 해당 프로퍼티를 오버라이드하여 고유 크기를 커스텀할 수 있다.
     
     - width 또는 height에 `UIView.noIntrinsicMetric` 프로퍼티를 넘겨주어 해당 방향에 대해서는 고유 크기가 결정되지 않는다는 것을 알릴 수 있다.
     
   - Content Hugging 및 Compression Resistance
     - Content Hugging : 크기가 커지기 어려운 정도 (컨텐츠를 끌어 안음. 컨텐츠 안으로 작용하는 힘)
     - Compression Resistance : 크기가 작아지기 어려운 정도  (압축 저항. 컨텐츠 밖으로 작용하는 힘)
     - Intrinsic Content Size가 결정된 방향에 대한 제약을 추가하지 않는 경우 Intrinsic Content Size로 크기가 결정된다. (Intrinsic Content Size를 기본 제약으로 추가한다.)
       - `UILabel`의 Intrinsic Content Size가 `CGSize(width: 100, height: 50)`일 때
         - `label.width`
         - <= 100@250 (Content Hugging)
           - \>= 100@750 (Compression Resistance)
         - `label.height`
           - <= 50@250 (Content Hugging)
           - \>= 50@750 (Compression Resistance)
     - 인터페이스 빌더나 코드로 프로그래머가 설정한 제약의 기본 우선순위는 1000이므로, 일반적으로 프로그래머가 설정한 제약이 가장 높은 우선순위를 갖는다.
     - 그러므로 Content Hugging과 Compression Resistance는 컨텐츠에 따라 사이즈가 동적으로 변할 때, 다시 한번 너비와 높이의 제약을 새롭게 결정하고 싶지 않을 때 사용한다.
     
   - Intrinsic Content Size의 동적 변경
     - `UIView`는 `updateConstraints`를 호출한 직후 해당 프로퍼티를 참조한다.
     - 커스텀 뷰를 만들 때, Intrinsic Content Size를 변경할 필요가 있는 경우 먼저 `invalidateIntrinsicContentSize`를 호출하여 값을 다시 계산하도록 해야 한다.
     - `UILabel`의 경우 text가 변경될 때 `invalidateIntrinsicContentSize`를 호출한 후 Intrinsic Content Size를 다시 계산한다.
3. **updateViewConstraints**
   - `UIViewController`의 메소드
   - 뷰 컨트롤러의 뷰가 그 제약을 갱신할 필요가 있을 때 호출된다.
   - 오버라이드할 때, super 메소드를 호출한 후 루트 뷰의 서브뷰의 제약을 갱신하는 코드를 작성할 수 있다.
   - `updateConstraints`의 동작을 대신한다.

### Layout 관련

위에서 수행한 Constraints를 바탕으로 레이아웃을 설정한다. View의 center 및 bounds를 결정한다. superview에서 subview 순서대로 갱신된다.

1. **viewWillLayoutSubviews**
   - `UIViewController`의 메소드
   - 뷰 컨트롤러의 뷰가 그 서브뷰를 배치하려 하는 것을 알리기 위해 호출된다.
2. **layoutSubviews**
   - `UIView`의 메소드
   - 서브뷰를 배치한다.
   - 오버라이드할 때 반드시 super 메소드를 호출해야 한다.
   - 시스템이 적절한 타이밍에 호출해준다. 직접 호출해서는 안 된다.
     - 레이아웃 변경을 스케줄링하기 위해 `setNeedsLayout` 메소드를 직접 호출할 수 있다. 이 메소드는 호출되는 순간 레이아웃을 갱신하지 않고, 시스템이 한번에 정리하여 레이아웃을 갱신한다.
     - 시스템이 제약의 변경을 실행할 때 `layoutIfNeeded`를 호출한다. 이 메소드는 직접 호출할 수 있다.
3. **viewDidLayoutSubviews**
   - `UIViewController`의 메소드
   - 뷰 컨트롤러의 뷰가 그 서브뷰를 배치하였음을 알리기 위해 호출된다.

### Draw 관련

위에서 수행한 Layout 이후에 draw 작업을 한다. Core Graphics를 사용하여 그린다.

1. **draw**
   - `UIView`의 메소드
   - 넘겨진 직사각형 안에 리시버의 이미지를 그린다.
2. **viewDidAppear**
   - `UIViewController`의 메소드
   - 뷰 컨트롤러의 뷰가 뷰 계층에 추가되었음을 알린다.

### 정리

| 사이클      | 업데이트 (시스템 호출) | 마크 (업데이트 스케줄링)  | 트리거 (프로그래머 호출)  |
| ----------- | ---------------------- | ------------------------- | ------------------------- |
| Constraints | updateConstraints      | setNeedsUpdateConstraints | updateConstraintsIfNeeded |
| Layout      | layoutSubviews         | setNeedsLayoutSubviews    | layoutIfNeeded            |
| Draw        | draw                   | setNeedsDisplay           | X                         |

## 오토레이아웃과 애니메이션

프레임을 조정하는 것과는 다르게 코딩해야 한다.

1. 제약 갱신
2. 레이아웃 실행

```swift
widthConstraint.constant = 100
UIView.animate(withDuration: 0.5, animations: { self.view.layoutIfNeeded() })
```

레이아웃 사이클의 3단계 : 제약 -> 레이아웃 -> 그리기에 따라, 제약을 변경한 후 애니메이션 블록에 레이아웃 갱신 코드를 작성한다.

바로 레이아웃을 실행하기 원하므로 `layoutIfNeeded`를 호출한다.

## 코드로 오토레이아웃 구현

`UIView`의 `translatesAutoresizingMaskIntoConstraints`를 false로 설정해준다.

true일 때 오토리사이징 마스크의 값이 제약으로 자동 추가되어 프로그래머가 설정한 제약과 충돌할 수 있다.

인터페이스 빌더에서 오토레이아웃을 설정한 경우 시스템이 자동으로 해당 프로퍼티를 false로 설정해준다.

## 참고

https://itpeace.tistory.com/44