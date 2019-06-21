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

## A Note on Immutability

## Creating the Toggle Favorite Method

## Working on the Favorite Feature

## Adding "notifylisteners"

## Finishing the Favorite Feature

## Fixing a Bug related to the "Favoriting" Feature

## Adding a User Model

## Mixins & The Latest version of Dart

## Using Mixins to Merge Models

## Logging in with the Main & the User Model

## Connecting Models & Sharing Data

## Changing Mixins Syntax

## Improving the Code & Fixing an Error

## Unselecting the Product after Editing

## Wrap Up