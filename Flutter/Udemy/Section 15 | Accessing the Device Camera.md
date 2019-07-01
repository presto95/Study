# Accessing the Device Camera

## Module Introduction

Flutter에서 카메라 / 사진 갤러리 기능 사용하기

## Adding an Image Picker Button

OutlineButton 위젯을 사용하여 윤곽선 관련 속성을 내장한 버튼을 만들 수 있다. 윤곽선 속성(`borderSide`)는 BorderSide 클래스를 요구하며 색상, 스타일, 두께를 설정할 수 있다.

## Adding the "Use Camera" & "Gallery" Buttons

`showModalBottomSheet` 나 `showBottomSheet` 메소드를 통해 나타난 bottom sheet는 하단에서 화면의 일부를 차지하는 뷰가 새로 나와 일시적으로 어떠한 정보를 표시하거나 사용자 입력을 받을 때 사용될 수 있다.

## Connecting the "Choose" Buttons to the Image Picker

Flutter에서 사진 앱에 접근하여 이미지를 선택하고 카메라를 사용하여 사진을 촬영하는 기능을 구현하기 위해 `image_picker` 써드 파티 패키지를 사용한다.

## Adding an Image Preview

ImagePicker는 File 타입으로 사진 데이터를 반환하며, `Image.file` 생성자를 사용하여 File을 Image 타입으로 변환할 수 있다.

## Setting Up Firebase Cloud Functions

Firebase Functions 사용 (node.js 사용)

- 서버를 구축하지 않고 API 엔드포인트를 만들 수 있음
- 현재 진행중인 프로젝트 내에 functions 디렉토리를 만들어주며, index.js 파일에 자바스크립트 코드를 작성하여 API 구축

서버 단 코드를 작성하므로 정리하지 않는다.

## Configuring the Upload Request

`mime` 써드 파티 패키지를 사용하여 리소스의 mime 타입을 구하는 기능을 수행할 수 있다.

---

### MIME?

음악, 영상과 같은 바이너리 파일을 전달할 때 텍스트 파일로 변환하여 보내며, 텍스트 파일로 변환하는 것을 인코딩, 텍스트 파일에서 바이너리 파일로 변환하는 것을 디코딩이라고 한다.

이 때 해당 바이너리 파일이 어떠한 종류의 것인지 나타내는 것이 MIME이다.

헤더에 함께 달아 보내는 Content-Type은 MIME-Type의 종류 중 하나이다.

---

이미지와 같은 바이너리 파일을 네트워크를 통해 전송할 때는 일반적으로 multipart Content-Type을 사용한다.

```dart
// 이미지의 경로로부터 MIME-Type을 구한다. 
// `application/json`과 같은 형태이므로 `/`를 기준으로 문자열을 자른다.
final mimeType = lookupMimeType(image.path).split("/");
// 이미지 업로드 요청 객체를 만든다. 
// http 패키지의 `MultipartRequest` 클래스를 사용하며, 사용하는 메소드와 Uri를 넘겨준다.
final imageUploadRequest = http.MultipartRequest(
  "POST",
  Uri.parse("some_url"),
);
// 주어진 경로로부터 이미지 파일을 가져온다.
final file = await http.MultipartFile.fromPath(
  "image",
  image.path,
  contentType: MediaType(
    mimeType[0],
    mimeType[1],
  ),
);
// 이미지 업로드 요청 객체에 위에서 가져온 이미지 파일을 추가한다.
// 헤더, 필드 등도 추가할 수 있다.
imageUploadRequest.files.add(file);
if (imagePath != null) {
  imageUploadRequest.fields["imagePath"] = Uri.encodeComponent(imagePath);
}
```

나중에 이미지 관련 작업이 필요할 때 구글링을 통해서 처리하도록 하자.

## Setting Headers to Add the Token

요청 객체에 토큰을 포함하는 헤더를 추가하기

## Fetching & Using Images 

## Previewing & Editing the Image

## Adding the Image Upload Flow

## Deleting Images When Deleting a Product

## Wrap Up