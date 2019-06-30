# Adding Google Maps to our App

## Module Introduction

Flutter에서 위치 기반 기능 사용하기 (지도 / 현재 위치 등)

Google 지도를 띄우기 위해 `map_view` 써드 파티 패키지를 활용한다.

위치 정보 기능을 사용하기 위해 `location` 써드 파티 패키지를 활용한다.

## Requesting Permissions

`map_view` 리드미를 따라 환경 설정하기

## Preparing our Text Form Field

어떠한 컨트롤러에 등록한 리스너는 위젯이 메모리에서 해제될 때에도 남아있으므로 리스너를 제거하는 코드를 넣어주는 것이 좋다.

일반적으로 `dispose` 메소드에 리스너를 지우는 코드를 작성한다. 

## Adding a Static Dummy Map

### Static Map

map_view 패키지의 클래스들을 사용하여 provider와 uri를 생성할 수 있다.

```dart
final provider = StaticMapProvider(key);
final staticMapUri = provider.getStaticMapUriWithMarkers(
  [Marker("id", "name", latitude, longitude)], 
  center: Location(latitude, longitude), 
  width: 100, 
  height: 100, 
  maptype: StaticMapViewType.roadmap,
);
```

`staticMapUri` 를 `Image.network` 생성자에 넘겨주면 주어진 정보에 기반한 정적 지도 이미지를 받을 수 있다.

## Sending a Request to Convert an Address to Coordinates

Google의 Geocoding API 사용하여 주소를 좌표로 변환하기

TextField나 TextFormField의 `controller` 를 명시적으로 지정하여 필드에 입력된 텍스트를 얻거나 텍스트의 변화에 따른 작업을 할 수 있다.

FocusNode에 리스너를 달아 특정 처리를 할 수 있다. 특정 텍스트 필드에 focus node를 넘겨주고 포커스를 잃을 때마다 특정 작업을 하도록 할 수 있다.

```dart
@override
void initState() {
  super.initState();
  _addressInputFocusNode.addListener(_doSomething);
}

void _doSometing() {
  if (_addressInputFocusNode.hasFocus) {
    // do something
  }
}
```

Uri 클래스의 `https` 및 `http` 정적 메소드를 사용하여 경로 전체를 uri로 사용하는 대신 쿼리, 도메인 등을 사용하여 엔드포인트 경로를 만들 수 있다.

## Adding Geocoding & Maps with Real Coordinates

Geocoding API가 내려주는 응답을 잘 골라내어 주소와 위경도 정보를 얻는다. 위경도 정보는 Static Map API 호출에 사용된다.

## Working on the Map Control

주소를 검색할 때 유효한 장소 정보가 내려오지 않는 경우에 대응한다.

## Storing Location Data in the Database

데이터베이스에 저장할 때는 데이터베이스가 지원하는 타입으로 변환하여 저장해야 한다.

클라언트에서 데이터를 주고받을 때는 클래스 등으로 정의한 타입을 사용하는 것이 좋으며, 네트워킹을 통해 서버로 데이터를 보내는 시점에서는 데이터베이스가 지원하는 타입으로 변환하여 실어 보내야 한다.

## Loading Location Data from the Backend

네트워킹을 통해 서버 데이터베이스로부터 좌표 정보를 가져오고 초기 상태로 사용하여 UI 갱신하기

## Updating an Existing Product's Position

기존 update 메소드에 위치 정보를 함께 넘겨주어 서버 데이터베이스의 위치 관련 정보도 갱신할 수 있도록 하기

## Adding the Location Package

`location` 써드 파티 패키지를 사용하여 위치 정보 기능을 사용한다.

## Using the Location Package with Version >=1.4

## Getting the User Location

## Preventing Memory Leaks

## Displaying the Address

## Showing a Fullscreen Map

## Wrap Up