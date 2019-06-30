# Authentication

## Module Introduction

앱은 인가된 사용자만 기능을 사용할 수 있도록 해야 할 필요가 있다.

Firebase 인증 API 사용하여 회원가입, 로그인 기능과 같은 인증 기능을 구현한다.

## How Authentication Works

1. 클라이언트는 서버에 인증과 관련된 정보를 보낸다.
2. 서버는 해당 정보가 데이터베이스에 있는지 확인하고 적절한 응답을 내려준다.
   - 토큰 등을 내려줄 수 있다.
3. 클라이언트는 응답에 따라 사용자 인가 처리를 한다.
   - 내려온 토큰을 저장하여 활용한다.
4. 서버에 리소스를 요청할 때 토큰을 활용한다.

## Adding a "Confirm Password" Textfield

---

### Dart

#### 열거형

```dart
enum AuthMode {
  signup,
  login
}
```

Dart의 열거형은 위와 같이 정의한다. Swift처럼 열거형에 메소드나 프로퍼티를 정의할 수 없다.

---

위젯 트리에 null이 넘어가면 에러가 발생한다. 조건적으로 특정 위젯을 빌드해야 한다면 조건이 맞지 않을 때 빈 컨테이너 (`Container()`) 를 넘겨주도록 한다.

## Implementing the Signup Functionality

http 패키지를 사용하여 서버에 요청을 보낸다.

`body` 프로퍼티에는 Map 데이터가 아닌, convert 패키지를 사용하여 JSON 포맷으로 인코딩한 데이터를 넘겨주어야 한다.

## Handling Errors

---

### Dart

Map 타입의 `containsKey` 또는 `containsValue` 메소드를 사용하여 해당 데이터가 특정 키나 값을 갖고 있는지 쉽게 확인할 수 있다.

---

서버가 요청에 대해 응답을 내려주는 것을 실패하거나 서버의 로직에 의해 에러가 발생한 응답을 내려주는 것을 클라이언트가 인지하여 적절하기 처리할 수 있어야 한다.

상태 코드, 에러 메세지 등으로 분기, 여러 에러 상황에 대해 적절하게 처리하여 사용자가 혼란스러워하지 않도록 해야 한다.

## Adding a Spinner Whilst Signing Up

UX 개선 요소

진행 중 상태와 관련된 상태값을 하나 정의하고 이를 활용하여 프로그레스 인디케이터를 띄우거나 띄우지 않게 한다.

## Authenticating Requests with Tokens

회원가입 또는 로그인 성공시 서버가 내려주는 토큰을 서버 API를 호출할 때 함께 보낸다.

## Storing the Token on the Device

토큰을 디바이스에 저장하여 토큰이 유효할 때 자동 로그인 등을 할 수 있게 하여 사용자 경험을 좋게 할 수 있다.

`shared_preferences` 써드 파티 패키지를 사용하여 간단한 키-값 쌍을 저장할 수 있다.

- iOS의 UserDefaults 사용
- Android의 SharedPreferences 사용

민감한 정보를 담기에는 부적절하다.

## Signing Users In Automatically

이미 로그인되어 토큰을 기기가 가지고 있다면 이후 실행시 인증 과정을 생략할 수 있도록 한다. 

## Adding a Logout Button

디바이스에 저장된 사용자 관련 정보를 초기화하고 인증 화면으로 리다이렉트하는 것으로 로그아웃을 진행할 수 있다.

## Adding Autologout

일반적으로 토큰은 일정 시간 후에 만료되며 리프레쉬 토큰을 활용해야 할 필요가 있다.

---

### Dart

#### Timer 클래스

Timer 클래스를 사용하여 일정 Duration 이후에 특정 함수를 실행하도록 할 수 있다.

```dart
Timer(Duration(seconds: 1), () { /* do something */ });
```

Swift의 Timer 클래스와 비교 가능하다.

#### DateTime 클래스

DateTime 클래스는 현재 지역에서의 시간과 날짜를 표현하는 클래스이며, 제공하는 메소드나 프로퍼티를 사용하여 타임스탬프를 만들 수 있다.

- `DateTime.parse` 정적 메소드를 사용하여 ISO8601 문자열을 DateTime 클래스로 파싱할 수 있다.
- `DateTime.fromMillisecondsSinceEpoch` 정적 메소드를 사용하여 밀리세컨드로 표현된 타임스탬프 숫자를 DateTime 클래스로 파싱할 수 있다.
- 이외에 특정 DateTime과의 차이, 이전 또는 이후 판별 등의 기능을 사용할 수 있다.

Swift의 Date 클래스와 비교 가능하다.

---

## Route Protection & Redirection

### RxDart

비동기 처리를 쉽게 하기 위해 RxDart 써드 파티 패키지 사용하기

- PublishSubject
  - `add` 메소드 사용하여 subject에 데이터 보내기
  - `listen` 메소드 사용하여 subject 구독하기
    - `onData` / `onError` / `onDone` 메소드를 받아 각각의 이벤트를 처리할 수 있다.

토큰 만료시 인증 화면으로 리다이렉트시키기 위해 해당 이벤트를 받아 특정 행위를 하도록 한다.

MaterialApp의 `onGenerateRoute` 에서 스트림에서 나온 이벤트에 따라 동적으로 페이지를 만들어 리다이렉트할 수 있다.

상황에 따른 여러 개의 MaterialApp을 정의하여 띄울 수 있다.

Stateful Widget의 상태 변경에 따른 생명 주기와 페이지 이동 로직을 이해하여 해당 기능을 구현할 수 있도록 한다.

## Fixing the Manual Logout

MaterialApp의 `routes` 와 `onGenerateRoute` 에 로직을 잘 정의하여 외부 위젯에 내비게이션 코드를 작성하는 대신 상태에 따라 자동으로 내비게이션되게끔 할 수 있다.

## Time for a Quick Recap Regarding our Code Structure

### scoped_model vs. rxdart

Scoped Model 개념을 사용하면 사소한 상태 변경에도 모든 위젯 트리를 다시 빌드해야 하므로, Flutter가 그 차이를 계산하여 성능상 최적화를 한다 할지라도 분명히 부담이 되는 경우가 있을 것이다.

Reactive Programming의 개념 (RxDart) 을 사용하면 위젯 트리를 다시 빌드하지 않고 비동기적으로 나온 상태에 따른 처리를 하면 된다.

## Adding Optimistic Updating to Store the Favorite Status

PUT 메소드 사용하여 좋아요 상태 변화 때마다 서버에 있는 데이터베이스 정보 변경하기

게시물 데이터베이스에 해당 게시물을 좋아하는 유저 id를 기록할 수 있도록 한다.

## Fetching the Favorite Status

서버 데이터베이스에 좋아요 관련 정보가 들어갔으므로 이를 받아 UI를 업데이트한다.

null 체크 `?.` 연산자 활용

## Allow Editing for own Posts Only

서버 데이터베이스의 유저 id 값을 활용하여 특정 사용자가 업로드한 정보만 취할 수 있도록 한다.

## Wrap Up

- 인증 기능을 사용하여 인가된 사용자만 정보에 접근할 수 있도록 한다.
  - 서버 단 로직에 의해 진행되며, 클라이언트는 응답을 받아 UI를 갱신하는 정도의 역할을 한다.
- 토큰을 활용한 인증
  - 서버 API 요청시 헤더에 토큰을 붙여 인가된 사용자에 의한 요청인지 검증할 수 있도록 한다.
  - 토큰 만료시 로그아웃될 수 있도록 한다.
- RxDart 및 Scoped Model
  - Scoped Model은 `notifyListeners()` 메소드가 호출될 때마다 위젯 트리를 다시 빌드한다.
  - RxDart는 특정 이벤트 발생시 특정 처리만을 수행한다.