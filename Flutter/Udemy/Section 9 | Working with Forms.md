# Working with Forms

## Module Introduction

사용자의 입력을 능률적으로 처리하기

## Using the Form Widget

Form은 여러 개의 연관된 입력 필드를 관리하는 것을 편리하게 해준다.

Form 위젯은 `child` 로 해당 폼에서 관리하기 원하는 입력 필드를 요구한다.

Form의 기능을 활용하기 위해서는 텍스트 입력를 위해서 TextField 대신 TextFormField를 사용해야 한다.

Widget의 `key` 프로퍼티에 키를 등록하여 앱의 다른 장소에 있는 위젯들과 상호작용할 수 있게 할 수 있다.

```dart
final GlobalKey<FormState> _formKey = GlobalKey<FormState>();
```

`_formKey` 를 Form 위젯의 `key` 프로퍼티에 넘겨주어 앱의 다른 장소에서 이를 식별할 수 있게 한다.

제출하는 버튼을 눌렀을 때 `_formKey.currentState.save();` 를 통해 상태를 저장할 수 있고, Form으로 래핑되어 있던 TextFormField의 `onSaved` 프로퍼티에 등록된 콜백 함수가 실행된다.

## Adding Form Validation

Form은 유효성 검사를 하는데 큰 이점을 갖는다.

TextFormField는 `validator` 프로퍼티를 갖는다. 이것에 유효성 검사와 관련된 콜백 함수를 등록할 수 있다.

- 입력된 텍스트가 유효하면 콜백 함수가 null을 반환하도록 한다.
- 입력된 텍스트가 유효하지 않으면 콜백 함수가 에러 메세지 (임의의 String) 을 반환하도록 한다.

`validator` 프로퍼티에 등록된 콜백 함수는 `_formKey.currentState.validate();` 의 코드를 통해 수행될 수 있다. `validate()` 는 true나 false를 반환하므로 이를 사용하여 분기 처리한다.

`autovalidate` 프로퍼티를 true로 해주면 텍스트 필드에 변화가 일어날 때마다 `validator` 를 수행하여 텍스트 필드의 에러 메세지가 뜨는 부분에 에러 메세지를 표시하거나 표시하지 않는다.

> Form을 사용하는 것은 지금 상태에서는 외워야하는 부분이 있는 것 같다.
>
> 원래 하던 것처럼 상태에 각 텍스트 필드에 대한 텍스트를 저장하고 제출 시 유효성 검사를 하도록 할 수 있으나, Form을 사용하여 Flutter가 제공하는 방법을 사용할 수도 있다.

## Advanced Validation

RegExp 클래스를 사용하여 유효성 검사에 정규 표현식을 활용할 수 있다. 해당 클래스의 다양한 메소드와 프로퍼티를 활용하여 입력이 정규 표현식에 적합한지 확인할 수 있다.

## A Note on the "Price" Input and the Usage of Commas / Dots

유럽에서 `9,99` 처럼 작성된 숫자는 소수가 맞으나 double로 파싱되지 않고 `^(?:[1-9]\d*|0)?(?:[.,]\d+)?$` 와 같은 정규 표현식을 사용하여 걸러지지 않는다.

`double.parse(value.replaceFirst(RegExp(r","), "."));` 와 같은 코드를 작성하여 `9,99` 를 `9.99` 로 바꾼 후 double로 파싱할 수 있다.

## Closing the Keyboard

키보드 다루기.

텍스트 필드와 같은 포커스를 가질 수 있는 위젯은 Focus Node를 갖고 있다.

텍스트 필드에 포커스가 위치하여 키보드가 올라와 있다면 포커스를 없애는 것으로 키보드를 닫을 수 있다.

```dart
onTap: () {
  FocusScope.of(context).requestFocus(FocusNode());
}
```

임의로 FocusNode 오브젝트를 만들고 그것에 포커스를 요청하여 기존에 존재한 포커스를 다른 곳으로 돌린다.

iOS의 Respodner Chain, `resignFirstResponder()` 등과 비교 가능하다.

## Submitting Data

Notify the framework that the internal state of this object has changed.

`setState()` 메소드는 State 오브젝트의 내부 상태에 변화가 일어났을 때 Flutter 프레임워크에게 알리는 역할을 한다.

내부 상태에 따라 `build()` 메소드가 호출되어 UI를 다시 그려야 하는 경우 등에 활용하며, 내부 상태를 변경하는 것에만 관심이 있다면 `setState()` 메소드를 작성할 필요가 없다.

## Outputting Lists of Products



## Re-Using the Product Create Form

## Setting Initial Values

## Updating Products

## Ensuring Input Visibility

## Wrap Up