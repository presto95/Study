# Handling User Input

## Module Introduction

사용자 입력 처리하기.

## Saving User Input

Material 패키지는 TextField 위젯을 제공한다. 

- 텍스트 필드에 변화가 있을 때, 또는 제출될 때 수행할 콜백 함수를 등록할 수 있다.
- 최대 라인 수, 키보드 타입, 자동 완성 여부, 뷰 이동시 포커스 여부 등을 설정할 수 있다.
- iOS의 `UITextField` 와 비교 가능하다.

## Configuring Text Field

여러 개의 TextField를 넘겨주고 각각의 텍스트 값을 내부 상태로 저장하여 처리할 수 있다.

## Styling Text Fields

TextField의 스타일과 관련된 프로퍼티

- `decoration` : InputDecoration 클래스 사용
  - `labelText` : placeholder처럼 작동하다가 포커스가 주어지면 텍스트 필드의 상단에 작게 위치하게 됨
  - `hintText` : placeholder

## Fixing a Bug & Adding a Button

키보드가 화면을 가리는 경우가 있을 수 있으며, 텍스트 필드를 감싸는 컨테이너를 ListView로 바꾸어 해결할 수 있다.

## Using our Form to Create a New Product

---

### Dart

함수 타입을 명시하는 문법

| Dart                               | Swift                  |
| ---------------------------------- | ---------------------- |
| int Function()                     | () -> Int              |
| void Function()                    | () -> Void             |
| Map<String, dynamic> Function(int) | ([String: Any]) -> Int |

---

## Improving the Style of our Form

SizedBox 위젯을 사용하여 고정 너비와 높이를 갖는 빈 공간을 만들 수 있다.

---

### VSCode

command P 눌러서 빠른 파일 이동

---

## Fixing a Tiny "Error"

- 일단 프로퍼티나 메소드를 private 접근 수준으로 만들어 놓고 필요시 풀어주는 습관을 들이는 것이 좋다. 
  - 언더스코어를 사용하여 프로퍼티 및 메소드의 이름을 작성하자.

## Adding a Switch

Material 패키지는 Switch 위젯과 SwitchListTile 위젯을 제공한다.

- Switch는 말 그대로 스위치.
- SwitchListTile은 ListView의 리스트 타일로 사용될 수 있으며 좌측에 타이틀, 우측에 스위치가 위치한다.
- 내부 상태를 두고 그것을 변화시키는 방식으로 구현해야 한다.

## Wrap Up

- TextField 위젯을 사용하여 텍스트 필드 구현
- Switch와 같은, 위젯의 값 변화를 감지하고 해당 값을 다시 위젯에 넘겨주어야 하는 경우가 있다.