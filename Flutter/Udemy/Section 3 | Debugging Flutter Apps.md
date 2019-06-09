# Debugging Flutter Apps

## Module Introduction

디버깅을 잘 하는 것은 당연히 중요하다!

## Fixing Syntax Errors

Syntax Error : 구문 오류

문법적 오류 등을 찾아 컴파일 가능하게 한다. 쉽게 파악 가능하다.

## Understanding Runtime Errors & Runtime Messages

Runtime Error : 실행 중 발생하는 에러

콘솔에 출력된 에러 메세지를 참고하여 에러 해결하기

해결 방법을 제안하는 경우도 있다.

스택 트레이스를 쫓아가며 디버깅할 수도 있다.

## Dealing with Logical Errors

Logical Error : 논리 오류

실행에 이상이 없지만 개발자가 기대한 대로 동작하지 않음 -> 휴먼 에러

동작과 관련된 코드를 들여다보기

## Using Breakpoints

코드 한줄 한줄의 실행을 관찰하기 위해 브레이크 포인트를 활용할 수 있다.

VS Code의 디버깅 패널에서 변수 / 조사식 / 호출 스택 / 중단점을 잘 활용하면 좋다.

## Debugging the User Interface

`flutter/rendering.dart` 패키지를 import하여 개발시 UI와 관련된 디버깅을 하기 쉽게 해주는 기능을 사용할 수 있다.

- `debugPaintSizeEnabled` 프로퍼티를 true로 하여 디버깅 모드로 실행시 위젯의 컨텐츠에 대한 정보를 시각적으로 나타냄
  - `main` 함수의 구현에 해당 프로퍼티의 값을 할당해주는 것이 좋음
  - margin : 컨텐츠의 바깥으로 향하는 여백
  - padding : 컨텐츠의 안쪽으로 향하는 여백

## More About Visual Helpers

- `debugPaintBaselinesEnabled` 프로퍼티를 true로 하여 텍스트의 baseline을 표시할 수 있음
- `debugPaintPointersEnabled` 프로퍼티를 true로 하여 탭 행위를 기대한 위젯에 대한 디버깅을 할 수 있음
- MaterialApp의 `debugShowMaterialGrid` 프로퍼티를 true로 하여 격자 모양을 스크린에 나타낼 수 있음

## Wrap Up

- 여러 상황의 에러를 활용 가능한 툴을 사용하여 현명하게 해결하자.
- UI 디버깅시 활용 가능한 것들을 알아두자.
- Flutter와 각 IDE의 공식 사이트에서 확인할 수 있는 디버깅 관련 문서를 읽어보자.

