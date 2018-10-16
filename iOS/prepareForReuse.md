## `UITableViewCell` 의 재사용

### `prepareForReuse()`

#### 테이블 뷰의 델리게이트에 의해 재사용을 위한 재사용 가능한 셀을 준비한다

`UITableViewCell` 객체가 재사용 가능하면, 즉, *reuse identifier* 를 가지고 있다면, 이 메소드는 `UITableView` 의 `dequeueReusableCell(withIdentifier:)` 메소드로부터 객체가 반환되기 직전에 호출된다. 퍼포먼스적 이유로, 알파값, 편집, 선택 상태와 같은 컨텐츠와 관련된 것이 아닌 셀의 속성을 리셋해야만 한다. `tableView(_:cellForRowAt:)` 의 테이블 뷰 델리게이트는 **항상** 셀을 재사용할 때 모든 컨텐츠를 리셋한다. 셀 객체가 관련된 *reuse identifier* 를 가지고 있지 않다면, 이 메소드는 호출되지 않는다. 이 메소드를 재정의한다면, 상위 클래스 구현을 호출하는 것을 명확히 해야 한다.



`UITableViewCell` 클래스에 정의된 `prepareForReuse()` 메소드와 관련된 문서 내용이다.

*reuse identifier* 를 가지고 있다면, 따로 처리를 해주지 않아도 `tableView(_:cellForRowAt:)` 에서 셀 재사용 시 모든 컨텐츠를 리셋해주므로 재사용 시 데이터가 개발자가 원치 않는 셀에 표시되는 이슈가 없다.



## `UICollectionReusableView` 의 재사용

### `prepareForReuse()`

#### 뷰를 재사용하기 위해 준비하는 데 필요한 어떤 정리 작업을 수행한다

이 메소드의 기본 구현은 아무것도 하지 않는다. 하지만, 이 메소드를 재정의할 때, 아무튼 상위 메소드 구현을 호출하는 것을 추천한다.

`UICollectionViewCell` 과 같은 서브클래스들은 이 메소드를 재정의하고 관련 행동을 수행하기 위해 사용한다. 그러므로 당신의 서브클래스가 `UICollectionViewCell` 또는 다른 몇몇 중간 클래스로부터 상속받았다면, `super` 를 호출하는 것은 그 클래스가 부모의 행동을 하는 것을 명확하게 한다.

뷰가 사용을 위해 *dequeue* 되었을 때, 이 메소드는 상응하는 *dequeue* 메소드가 당신의 코드에서 뷰를 반환하기 전에 호출된다. 서브클래스들은 이 메소드를 재정의하여 프로퍼티들을 기본값으로 재설정하고 일반적으로 뷰가 다시 사용될 준비가 되게 하기 위해 사용한다. 당신은 이 메소드를 뷰에 어떤 새로운 데이터를 할당하기 위해 사용하지 말아야 한다. 그것은 당신의 데이터 소스 오브젝트의 역할이다.



`UICollectionViewReusableView` 클래스에 정의된 `prepareForReuse()` 메소드와 관련된 문서 내용이다.



`UICollectionViewCell` 은 `UICollectionReusableView` 클래스를 상속받아 `prepareForReuse()` 를 사용할 수 있고, 이 메소드를 재정의하여 셀 재사용 시 적절한 처리를 해줄 수 있다.

여기서는 `UITableViewCell` 과는 다르게 기본적으로 셀의 컨텐츠들을 리셋해주는 것이 없으므로 `UICollectionViewCell` 를 사용할 때는 `prepareForReuse()` 메소드를 반드시 재정의하여 적절한 처리를 해줄 필요가 있겠다.