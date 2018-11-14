## MVC

### Model

- '무엇'에 해당하는. UI와 독립적인 객체들
- Controller에 대한 통신은 숨겨져 있음
  - Notification / KVO를 사용하여 모델에 변화가 발생했음 Controller에게 알림
- View에 접근할 수 없음 (Model은 View와 독립적 / View는 UI 관련된 것을 가지고 있으므로 UI와 독립적인 Model이 UI에 의존하는 View와 통신하는 것은 불가능 / View는 일반적인 객체이므로 '무엇'을 알 수 없음)
- **What** your application is (but not **how** it is displayed)

### View

- Controller에 대한 통신은 숨겨져 있고 구조적이어야 함. View는 일반적이므로 Controller가 무엇을 하는지 알 수 없음
  - Target-Action : Target을 Controller로 설정하고 View는 Target에 Action을 보냄
  - Delegate Pattern : Controller는 View에게 자신이 View의 델리게이트임을 알려줌. View는 Controller가 이를 어떻게 구현하는지 모름. 단지 Controller가 델리게이트를 구현한다는 것만을 앎
- Model에 접근할 수 없음 (Model은 View와 독립적 / View는 UI 관련된 것을 가지고 있으므로 UI와 독립적인 Model이 UI에 의존하는 View와 통신하는 것은 불가능 / View는 일반적인 객체이므로 '무엇'을 알 수 없음)
  - View가 표시하고 있는 데이터를 View의 프로퍼티로 가지고 있으면 안됨
  - TableView는 앱이 무엇인지, 데이터가 어떤 것인지 모르지만 받은 데이터를 뿌려줌
- Controller's minions. 컨트롤러의 하인

### Controller

- Model에 항상 접근할 수 있음. '무엇'이 어떤 것인지에 대해 사용자에게 보여주어야 하므로
- View에 항상 접근할 수 있음 (IBOutlet)
- Model의 정보를 해석하고 구성하여 View에게 전달
- View의 사용자와의 상호작용을 Model에게 해석해줌
- **How** your Model is presented to the user (UI logic)

하나의 MVC는 하나의 스크린 (하나의 UI 그룹)을 제어하고, 여러 개의 MVC가 모여 애플리케이션을 구성함

하나의 MVC 구조는 다른 MVC 구조에 접근할 때 Controller에 접근. 그래야 일반적이고 재사용 가능한 컴포넌트를 구성할 수 있음

---

## Model 만들기

새로운 클래스를 만들고 나면, 항상 이것의 **공개 API가 무엇인지** 생각해야 한다.

### 구조체와 클래스의 차이

- 구조체는 상속을 받을 수 없음. 이것이 구조체를 좀 더 간단하게 함
- 구조체는 값 타입 / 클래스는 참조 타입
  - Swift의 값 타입 복사시 모든 내용을 전부 복사하지 않음. 내용이 변경되었을 때만 실제로 복사하도록 하는 방식을 취함
  - 참조 타입은 힙에 자료형이 담겨 있고 그 자료형에 포인터를 사용함

**카드에 표시되는 이모티콘 (이모지, 이미지 등)은 Model이 아니라 View의 관점에서 생각해야 한다!** 카드는 UI와 독립적이다.

- 이미지나 이모지 등 VIew에 보여줄 것은 Model을 어떻게 보여줄 것이냐와 관련되어 있는 것
- Model은 단지 카드가 무엇을 해야 하는지에 관해서만 알고 있어야 한다.

구조체는 다른 변수에 할당할 때 복사됨. 배열에 넣거나 뺄 때도 복사됨.

```swift
// 총 세 개의 card가 있게 되는 것.
let card = Card()
cards.append(card)
cards.append(card)
```

타입 메소드 및 타입 프로퍼티는 타입 내에 정의되어 유틸리티 등의 역할을 수행할 수 있음

`lazy`를 명시한 변수는 접근될 때까지 초기화되지 않음

- 프로퍼티 감시자를 정의할 수 없음

for문으로 배열을 순차 탐색할 때 인덱스와 함께 도는 방법

- `for (index, element) in array.enumerated()`
- `for index in array.indices`
- `for index in 0..<array.count`

