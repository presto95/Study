# Working with Lists & Conditionals

## Creating Scrollable Lists with "ListView"

iOS의 `UITableView`와 Flutter의 `ListView`를 비교할 수 있다.

화면의 기본 경계를 초과하는 컨텐츠를 표시해야 하는 경우 스크롤 가능한 컨테이너에 이를 표시하도록 하는 것이 좋다.

ListView를 사용하기 위해 스크롤 가능한 영역을 지정해줄 필요가 있다.

- ListView를 Container에 감싼 후 `height` 를 지정하거나
- Expanded에 감싸 여분의 공간을 모두 ListView가 사용할 수 있도록 한다.

## Optimizing the List Loading Behaviour

ListView의 성능 최적화

ListView의 `children` 프로퍼티에 모든 아이템에 대한 위젯을 할당하는 경우 모든 리스트를 생성하는 것이므로 데이터가 많은 경우 비효율적이다.

`ListView.builder` 생성자를 사용하여 iOS의 셀 재사용과 같은 기법을 Flutter에서도 사용할 수 있다.

- `itemBuilder` 프로퍼티와 `itemCount` 프로퍼티를 기본적으로 활용한다.
  - iOS에서 `UITableViewDataSource` 의 `cellForRowAt` 과 `numberOfRows` 를 필수로 요구하는 것과 비슷하다.
  - `itemBuilder` 프로퍼티에는 BuildContext와 index를 나타내는 두 개의 인자가 있는 익명 함수를 넘겨준다.

## Rendering Content Conditionally

Dart의 모든 객체는 클래스, 참조 타입이므로 초기화가 되지 않았다면 null을 갖는다.

null 체크를 통해 적절한 처리를 해줄 수 있다.

화면에 표시할 데이터가 없는 경우 (null인 경우) 분기를 통해 UX적으로 적절한 처리를 해줄 수 있다.

## Alternative Approaches to Render Content Conditionally

어떠한 방식이든 분기만 잘 해주면 된다.

모든 위젯은 Widget 클래스로부터 파생되었으므로 Widget 타입에 할당 가능하다.

Widget을 반환하는 메소드를 정의하여 코드의 가독성을 높일 수도 있다.

## One Important Gotcha

UI에 대하여 null을 반환하게 되면 에러를 낼 수 있다. 빈 Container를 반환하는 것으로 해결할 수 있을 것이다.

## Wrap Up

- 스크롤 가능한 컨텐츠를 나타내기 위해 ListView를 사용한다.
  - 고정 높이를 가진 컨테이너 또는 Expanded 위젯에 감싸야 한다.
- `ListView(children:)` 는 정적 리스트를 표현할 때 사용하기 좋다.
- `ListView.builder` 는 동적 리스트를 표현할 때 사용하기 좋다. 웬만하면 이것을 사용하기

---

- ListView의 `scrollDirection` 프로퍼티에 Axis 인스턴스를 넘겨주어 스크롤 방향을 변경할 수 있다.
- GridView를 사용하여 iOS의 `UICollectionView` 와 비슷한 UI를 나타낼 수 있다.
  - `GridView.count` 생성자를 사용하여 스크롤 방향에 따른 칼럼의 개수(`crossAxisCount`), 하위 위젯들(`children`)을 명시하여 격자 모양의 UI를 만들 수 있다.