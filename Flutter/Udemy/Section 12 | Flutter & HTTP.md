# Flutter & HTTP

## Module Introduction

Flutter에서 네트워킹하기

## Understanding the Backend Setup

이 강좌에서는 Firebase Realtime Database를 사용하여 백엔드 구성

## Sending a POST Request

Flutter는 http 통신 관련 기능을 제공하지 않으며, `http` [패키지](https://pub.dev/packages/http#-readme-tab-)를 설치해야 한다.

```dart
import "package:http/http.dart" as http;
```

위의 코드처럼 패키지를 import하면 `http` 를 통해 패키지 기능에 접근할 수 있다.

GET, POST, FETCH, DELETE 등 각각의 http method로 요청할 수 있는 메소드를 제공한다.

- `Future<Response>` 객체를 리턴하여 `then` 메소드로 체이닝하여 응답을 처리할 수 있다.

Dart의 convert 패키지를 사용하여 Map의 json 인코딩 등의 기능을 사용할 수 있다.

- `json.encode(map)`

```dart
http.post("some_url", body: json.encode(map)).then((response) { /* ... */ })
```

JSON 인코딩된 map 데이터를 HTTP Body에 넣어 `some_url` 로 POST 요청하며, 요청이 완료되어 응답이 오면 `then` 메소드에 등록된 익명 함수에서 응답을 처리한다.

## Using the Response

Swift의 `URLSession` 의 경우 컴플리션 핸들러를 통해 응답을 처리하는 반면, Dart의 http 패키지에 있는 네트워킹 요청 관련 메소드는 Future 객체를 활용하여 응답을 처리한다.

Response 객체의 `body` 프로퍼티에서 응답 데이터를 얻을 수 있고, 이를 먼저 JSON 포맷으로 디코딩할 필요가 있다.

- `json.decode(response.body)`
- dynamic 타입을 반환하므로 기대되는 Map 형태의 타입을 갖도록 명시할 필요가 있다.

## Getting Data From a Server

GET 메소드를 사용하여 서버로부터 데이터를 가져오기

```dart
http.get("some_url").then((response) { /* ... */ })
```

## Transforming & Extracting Response Data

---

### Dart

Dart에서는 약어에 대하여 같은 케이스를 적용하지 않는 것이 언어가 제안하는 컨벤션이다.

- ex) json, Json
- Swift에서는 json, JSON처럼 같은 케이스를 적용한다.

---

네트워크 상에서 이미지를 불러오는 경우

- `Image.network` 생성자를 사용하여 url을 넘겨주어 이미지 불러오기
- `NetworkImage` 클래스 사용하여 url을 넘겨주어 이미지 불러오기
- 위의 작업들은 비동기적으로 처리된다.

로컬 에셋에서 이미지를 불러오는 경우

- `Image.asset` 생성자를 사용하여 에셋 이름을 넘겨주어 이미지 불러오기
- `AssetImage` 클래스를 사용하여 에셋 이름을 넘겨주어 이미지 불러오기

## Displaying a Loading Spinner

네트워킹과 같은 작업 소요 시간이 긴 것에 대해서 spinner(Activity Indicator)를 띄워 사용자에게 앱에 오류가 발생하지 않고 작업이 진행중이라는 것을 나타낼 수 있다.

이 상태가 중간에 바뀌었다고 할지라도 `notifyListeners()` 와 같은 UI를 다시 그리게 하는 메소드가 호출되기 전까지는 UI 상에 효력이 없다.

CircularProgressIndicator는 iOS의 `UIActivityIndicatorView` 와 비교 가능하며, 위젯 그 자체가 isLoading과 같은 상태를 가지지 않으므로 외부에서 해당 상태를 활용하여 위젯을 교체하는 (다시 그리는) 방식으로 구현한다.

## Finishing the Loading Spinner

null 값을 갖는 객체에 접근하여 프로퍼티나 메소드에 접근할 때 에러가 발생하므로 null 체크를 해주는 것이 중요하다.

컴플리션 핸들러의 행위를 구현하고 싶다면 메소드가 `Future` 를 반환하도록 하여 구현할 수 있다.

- 특정 값을 필요로 하지 않는다면 `Future<Null>` 타입을 반환하게 하여 구현할 수도 있다.
- 물론 Swift에서 클로저를 활용하여 컴플리션 핸들러를 구현하는 것처럼 Dart에서도 똑같이 구현할 수 있겠다.

```dart
// Dart, Future
Future<Null> addProduct(Product product) { 
  // ...
  return Future.value(null);
}
addProduct(product).then((_) { ... })
```

```swift
// Swift, Completion Handler
func addProduct(product: Product, completion: () -> Void) { 
  // ... 
  completion()
}
addProduct(product) { ... }
```

```dart
// Dart, Completion Handler
void addProduct(Product product, void Function() completion) {
  // ...
  completion()
}
addProduct(product, () { ... })
```

Future의 제네릭 타입을 달리 하여 작업 완료 후 특정 값과 함꼐 보낼 수 있다. 컴플리션 핸들러에 인자가 더해지는 것과 같다.

UI와 관련된 상태가 변화할 때 `notifyListeners()` 나 `setState` 와 같은 메소드를 호출하는 것을 잊지 않아야 한다.

## Updating Products

PUT 메소드는 서버 상의 데이터의 갱신과 관련 있다.

Create: POST / Read: GET / Update: PUT / Delete: DELETE

생성자를 통해 넘어오는 데이터를 저장하는 변수를 언더스코어 처리 해줄 필요는 없다.

## Deleting Products

DELETE 메소드는 서버 상의 데이터 삭제와 관련 있다.

UI가 갱신되는 시점, 상태가 변경되는 시점을 잘 동기화해야 한다.

## Using Pull to Refresh

RefreshIndicator 위젯에 pull 되는 컨텐츠를 감싸 구한다.

- `onRefresh` 프로퍼티에 해당 행위가 트리거되었을 때 실행할 콜백 함수를 등록한다.

## Adding "fadein" to the Image Placeholder

이미지가 로딩되기 전 플레이스홀더 이미지 나타내기

FadeInImage 위젯에 감싸 플레이스홀더, 이미지 로딩 완료시 플레이스홀더 이미지에서 로딩된 이미지로 전환시 애니메이션 등을 설정할 수 있다.

```dart
FadeInImage(
  image: NetworkImage("some_url"),
  placeholder: const AssetImage("some_name"),
	fit: BoxFit.cover,
  fadeInCurve: Curves.easeInOut,
  fadeInDuration: const Duration(milliseconds: 500),
)
```

BoxFit 열거형은 iOS의 `UIView.ContentMode` 열거형과 비교 가능하다.

| BoxFit  | UIView.ContentMode | 설명                                                         |
| ------- | ------------------ | ------------------------------------------------------------ |
| fill    | scaleToFill        | 컨텐츠의 종횡비를 왜곡하여 컨테이너를 채운다.                |
| contain | scaleAspectFit     | 컨테이너의 크기에 알맞도록 컨텐츠의 크기를 조절하여 채운다.  |
| cover   | scaleAspectFill    | 컨테이너를 가득 채우도록 컨텐츠의 크기를 조절하여 채운다. 컨텐츠는 컨테이너의 범위를 벗어날 수 있다. |

## Adjusting the Scoped Model & the Selected Product 

`firstWhere` 메소드를 사용하면 주어진 조건에 맞는 첫 번째 요소를 구할 수 있다. 찾지 못한다면 에러를 던지나 `orElse` 프로퍼티를 구현하여 에러 발생시 반환할 기본값을 정의할 수 있다.

`indexWhere` 메소드를 사용하면 주어진 조건에 맞는 첫 번째 요소의 인덱스를 구할 수 있다. 찾지 못했다면 -1을 반환한다.

데이터는 고유 식별자 (id 등) 에 의해 탐색되어야 한다.

## Handling Error Responses

네트워킹 에러에 대응하기

서버가 정상적으로 요청을 받아 응답을 내려준 경우라면 `Future<Response>` 에 값이 들어가서 응답이 내려오므로 상태 코드로 분기하여 에러를 처리한다.

네트워크 자체에 에러가 발생 (네트워크 연결 없음, 응답 시간 초과 등) 하는 경우 Future 객체에 에러 이벤트가 발생하고, 이 경우에  `catchError` 메소드로 체이닝하여 처리할 수 있다.

## Adding "async" "await"

비동기 메소드 만들기. `async` 키워드와 `await` 키워드를 사용한다.

```dart
Future<bool> addProduct(Product product) {
  http.post("some_url", body: json.encode(product))
    .then((response) {
      // do something
    }).catchError((error) {
    	// error handling
    });
}
```

```dart
Future<bool> addProduct(Product product) async {
  try {
    final response = await http.post("some_url", body: json.encode(product));
    // do something
  } catch (error) {
    // error handling
  }
}
```

Future의 메소드 체이닝을 통해서 비동기 작업을 수행할 수도 있고, async 및 await를 사용하여 비동기 작업을 수행할 수도 있다.

메소드 체이닝을 통한 작업은 콜백 함수를 등록하여 Future의 이벤트 발생을 감지하여 실행하고, async / await를 통한 작업은 await 줄에서 비동기적으로 작업이 완료되게 한 후 그 아래에 작성된 코드를 실행한다는 차이가 있다.

async / await를 사용하여 비동기 코드를 작성하면 마치 동기 코드처럼 보이면서 이해하기 쉬워지는 장점이 있다.

async 함수는 반드시 Future를 반환하도록 정의되어야 한다.

## Wrap Up

- `http` 패키지 사용하여 HTTP 통신 작업을 수행한다.
- 기본적으로 비동기적으로 수행된다.
  - Future 처리하기
    - `then` / `catchError` 메소드 체이닝
    - `async` / `await` / `try` - `catch`
- `convert` 패키지 사용하여 Map과 JSON 포맷의 변환 작업을 수행한다.
  - `json.encode(map)` / `json.decode(jsonObject)`
- 상태를 너무 이르게 재설정하거나 갱신하여 화면 전환시 에러가 발생하는 현상을 일으키지 않도록 한다.