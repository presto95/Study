# Models & State Management

## Module Introduction

능률적으로 데이터 및 상태 관리하기

## What can be Improved?

그동안 main.dart 파일에 데이터를 두고 하위 위젯에 이를 뿌려주는 식으로 데이터를 관리하는 것은 나쁜 접근은 아니지만 다른 좋은 방법이 있다.

그동안 데이터를 Map으로 관리했으나 문자열 기반으로 값을 빼오므로 좋지 않다. Model 클래스를 만들 필요가 있다.

## Adding a Product Model

데이터를 효율적으로 관리하기 위해 Model 클래스 작성하기.

Dart에 구조체는 없다.

```dart
import "package:meta/meta.dart";

class Product {
  final String title; 
  final String description;
  final double price;
  final String image;
  Product(
    {@required this.title,
    @required this.description,
    @required this.price,
    @required this.image}
  );
}
```

위와 같은 형태로 Product 모델을 정의할 수 있을 것이다.

`@required` 선언을 통해 positional argument를 필수적으로 넘겨야 하도록 만들 수 있다. `meta` 패키지를 import해야 사용 가능하다.

프로퍼티가 optional이 아닌 이상 `@required` 선언을 붙여주는 것이 좋겠다.

## Creating a Scoped Model

기존 main.dart 파일에서 데이터를 관리하고 데이터의 CRUD를 위한 메소드를 다른 위젯에 넘겨줌으로서 발생하는 문제

- 모든 다른 위젯의 생성자를 통해 데이터와 메소드를 넘겨주어야 함
- main.dart 파일이 너무 커짐

`scoped_model` 서드 파티 패키지 사용하기

- 각각의 위젯에 상응하는 각각의 상태 관리 모듈에 데이터를 주입하기
- 상태를 관리하는 모듈을 따로 빼내어 UI 갱신

`flutter packages get` 명령으로 패키지 수동 설치

---

### Dart

`get` 키워드를 사용하여 읽기 전용 프로퍼티 만들기

```dart
List<Product> _products;
List<Product> get products {
  return _products;
}
// 축약
List<Product> get products => _products;
```

`List.from` 생성자를 통해 List 복사하기

- Swift와 달리 Dart의 List는 참조 타입이므로 원본을 훼손시키지 않으려면 주소가 독립된 인스턴스를 만들어 주어야 한다.

---

## Connecting the Scoped Model

`ScopedModelDescendant`

- `builder` 프로퍼티에 할당된 콜백 함수는 scoped model에 변화가 생길 때마다 실행된다.
  - build context, widget, model을 인자로 갖는다.
  - 제네릭 타입으로 scoped model의 타입을 명시한다.

## Providing the Scoped Model

scoped model은 앱 전체에서 하나의 인스턴스로 존재해야 한다. 이는 일반적으로 싱글톤 패턴을 사용하여 해결할 수 있으나 `scoped_model` 패키지에서는 패키지가 제공하는 방법을 사용한다.

- 특정 scoped model을 사용할 위젯을 ScopedModel 위젯으로 감싼다.
  - `model` 프로퍼티에 사용할 모델의 인스턴스를 넘겨준다. 해당 인스턴스를 ScopedModel 위젯 계층 구조에 있는 모든 위젯에서 접근할 수 있다.

## Viewing Single Products

`build` 메소드에서 반환할 위젯을 `ScopedModelDescendant` 위젯에 감싸 앱 전체에서 공유되는 모델을 사용하여 UI를 만들 수 있다.

## Editing & Deleting Products with the Scoped Model

---

### Dart

getter 및 setter 작성하는 문법

```dart
int _selectedIndex;

int get selectedIndex => _selectedIndex;
set selectedIndex(int index) => _selectedIndex = index;
```

---

## Finishing the Product Model

`scoped_model` 서드 파티 패키지를 사용하여 상태 관리하기

- `Model` 클래스를 상속받는 scoped model 클래스는 앱의 일정 범위에서 동일하게 사용되도록 한다.

- `ScopedModel` 위젯의 계층 구조 상에서 하위에 위치한 위젯들은 이 위젯에 넘겨진 `model` 에 접근할 수 있다.

  - `ScopedModelDescendant` 위젯으로 감싸고 `builder` 프로퍼티에 콜백 함수를 등록하여 `model` 에 접근하여 UI를 그린다.

    - `notifyListeners()` 메소드가 호출되면 `buidler` 프로퍼티에 등록된 함수가 실행되어 UI를 다시 그린다.

  - scoped model 클래스에 헬퍼 메소드를 정의하여 접근할 수도 있다. 싱글톤 패턴과 비슷한 느낌

    ```dart
    class ProductModel extends Model {
      // ...
      static ProductModel of(BuildContext context) 
        => ScopedModel.of<ProductModel>(context);
    }
    ```

    - 이렇게 하면 `ScopedModelDescendant` 위젯에 감싸지 않아도 scoped model에 접근할 수 있다.
    - `rebuildOnChange` 프로퍼티의 기본값은 false이며, true로 해주어야 변화 발생시 rebuild 작업을 수행한다.

- 상태 변경 후 `notifyListeners()` 메소드를 호출하여 상태가 변경되었음을 알려야 한다. `setState` 의 역할을 한다.

자세한 사용법은 패키지의 [리드미](https://github.com/brianegan/scoped_model) 참고하기.

## A Note on Immutability

**불변성에 대하여**

Dart의 모든 타입은 레퍼런스 타입이므로 불변성에 특히 신경써야 한다.

기대하는 대로 외부에서 사용할 수 있도록 설계하여 API를 작성해야 한다.

```dart
List<int> numbers = [1, 2];
List<int> numbers2 = numbers;
```

위의 코드에서 `numbers` 와 `numbers2` 는 모두 같은 메모리를 가리킨다.

`numbers2` 에 접근하여 데이터를 가공한 경우 원본을 훼손하게 된다.

```dart
List<int> numbers = [1, 2];
List<int> numbers2 = List.from(numbers);
```

위의 코드에서 `List.from` 생성자를 통해 `numbers` 와 `numbers2` 는 값은 동일하지만 다른 메모리를 가리키게 된다.

코드에 불변성을 더하게 되면 해당 변수에 할당된 값이 변화하지 않는다는 것을 코드가 나타내게 되므로 동작을 이해하기 쉬워져 읽기 쉬워지고 유지보수성이 증가하게 된다.

## Creating the Toggle Favorite Method

모델의 프로퍼티가 불변하도록 하기 위해 `final` 로 선언해주므로, 갱신과 같은 작업을 하기 위해서는 새로운 객체를 만든 후 해당 위치에 대입하는 방식을 취해야 한다.

## Working on the Favorite Feature

`ScopedModelDescendant` 위젯에 감싸 scoped model을 사용하는 것, `ScopedModel.of<T>` 를 사용하여 scoped model을 사용하는 것.

특정 로직에서만 모델을 사용한다면 후자의 경우가 편할 수 있으나, 하나의 UI를 그리는 데 있어서 여러 프로퍼티가 모델을 사용한다면 전자를 취하는 것이 좋을 것이다.

## Adding "notifylisteners"

scoped model에 변경이 일어날 때마다 UI를 다시 그리도록 해주어야 한다. (`build` 메소드를 다시 호출한다.)

`notifyListeners()` 메소드는 scoped model 내부에서 호출하도록 구현하며, 외부에서 호출하지 않도록 한다.

## Finishing the Favorite Feature

```dart
List<Product> get displayingProducts
  => products.where((product) => product.isFavorite == showsFavorite).toList();
```

```swift
var displyingProducts: [Product] {
  return products.filter { $0.isFavorite == showsFavorite }
}
```

Dart와 Swift에서 익명 함수(클로저)를 작성하는 문법의 차이

`where` 메소드의 반환형은 `Iterable<E>` 이므로 `toList()` 를 통해 리스트로 바꾸어서 반환해준다.

프로퍼티에 기본값을 할당하지 않는 경우 null이므로 이러한 프로퍼티를 사용할 때 주의해야 한다.

## Adding a User Model

Product 모델처럼 사용자에 대한 모델도 작성해준다.

## Using Mixins to Merge Models

------

### Dart

**Mixin(믹스인)**

다른 클래스와 병합될 수 있는 클래스

어떠한 클래스의 기능을 다른 클래스에 제공할 수 있다.

`with` 키워드를 사용하여 어떠한 클래스가 with로 묶인 클래스의 기능을 사용할 수 있도록 한다.

- with로 묶인 클래스의 기반 기능을 사용할 수 없고 타입을 확장할 수 없으며 생성자를 호출할 수 없다.
- 단지 프로퍼티와 메소드를 병합하여 다른 클래스의 기능을 가져오는 것이다.
- 묶인 클래스의 프로퍼티나 메소드를 재정의하지 않도록 주의해야 한다.

상속은 수직적 관계로 자식 클래스가 부모 클래스의 모든 것을 가져오게 되지만 믹스인은 그저 다른 클래스의 기능을 가져올 뿐이다.

Swift의 프로토콜 초기 구현이 되어 구현을 가진 프로토콜과 비교할 수 있겠다. 어떠한 클래스의 프로퍼티와 메소드만을 취할 수 있기 때문이다.

#### 공식 문서

**믹스인: 클래스에 기능 추가하기**

믹스인은 여러 개의 클래스 계층 구조에서 클래스의 코드를 재사용하는 방법이다.

믹스인을 사용하려면 `with` 키워드 다음에 하나 이상의 믹스인 이름을 작성하라.

```dart
class Musician extends Performer with Musical { ... }
// Musician 클래스는 Performer 클래스를 상속받으며 Musical 클래스의 기능을 가져온다.
```

믹스인을 구현하려면 Object를 확장하는 클래스를 만들고 생성자를 선언하지 마라. 믹스인이 일반적은 클래스처럼 사용 가능하지 않게 하고 싶다면 `class` 키워드 대신 `mixin` 키워드를 사용하라.

```dart
mixin Musical {
  bool canPlayPiano = false;
  void entertainMe() {
    print("Playing Piano");
  }
}
```

오직 특정 타입만 믹스인을 사용할 수 있도록 특정하고 싶다면, 예를 들어 믹스인이 정의하지 않은 메소드를 호출할 수 있도록 하려면, `on` 키워드를 사용하여 요구 수퍼 클래스를 명시하라.

> 해당 믹스인이 믹스인될 수 있는 타입 제한

```dart
mixin MusicalPerformer on Musician { ... }
// MusicalPerformer 믹스인은 Musician 클래스에서만 믹스인될 수 있다.
```

```dart
class MainModel extends Model with ProductModel, UserModel { ... }
```

위의 코드는 `scoped_model` 패키지에 정의된 Model 클래스를 상속받으면서 ProductModel, UserModel의 기능을 가져와 사용 가능한 MainModel 클래스를 정의한다.

---

## Logging in with the Main & the User Model

로그인 정보를 User 모델에 담기

Main 모델은 Product 모델과 User 모델을 믹스인하여 그들의 기능을 갖게 됨

## Connecting Models & Sharing Data

## Improving the Code & Fixing an Error

---

### Dart

`_` 를 사용하여 프로퍼티나 클래스를 private하게 만드는 것은 사실 파일 내에서 private하게 만드는 것이며, Swift의 fileprivate 접근 수준을 갖는다고 생각하면 된다.

---

내비게이션 관련 코드(`Navigator.push`, `Navigator.pop`)는 Future를 반환하므로 `then` 메소드로 체이닝하여 작업 완료 후 실행할 코드 블록을 등록할 수 있다. 컴플리션 핸들러.

## Unselecting the Product after Editing

`notifyListeners()` 메소드를 호출하는 조건을 명확히 하여 시점을 잘 조절해야 애니메이션 중 거리낌 없는 UI를 만들 수 있다.

## Wrap Up

모델과 상태를 잘 관리하는 것은 매우 중요하다.

- 커스텀 타입 또는 모델로 사용하기 위한 클래스를 정의할 수 있다.
  - 실생활의 모델을 하나 이상의 타입으로 잘 녹여낼 수 있어야 한다.
- `scoped_model` 서드 파티 라이브러리를 사용하여 상태를 관리하는 방법을 공부하였다.
  - 생성자를 통해 데이터를 전달하는 방식을 사용하지 않을 수 있다.
  - 중앙에 위치하여 상태 (앱 전체적으로 사용되는 데이터) 를 관리할 수 있다.
    - 싱글톤 패턴 대체 가능
  - `ScopedModelDescendant` 위젯에 감싸 상태가 변화할 때 갱신되는 위젯 서브 트리를 만들 수 있다.
  - scoped model을 사용할 위젯 트리는 `ScopedModel` 에 감싸져야 한다.
    - `model` 과 `child` 를 받아 사용할 scoped model과 하위 위젯을 정의한다.
  - scoped model은 `Model` 을 상속받아야 한다.
  - 상태에 변화가 있을 때 `notifyListeners()` 메소드를 호출하여 `builder` 프로퍼티에 등록한 콜백 함수를 실행하여 UI를 다시 그리도록 한다.
- 여러 개의 scoped model을 mixin을 활용하여 조합할 수 있다.
  - 조합에 사용된 모델의 코드를 하나의 파일에 옮겨 `_` 을 사용하여 private한 접근 수준을 갖도록 할 수 있다.
  - 믹스인에 익숙해지기.

## Useful Resources & Links

