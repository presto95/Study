[참고 영상: Let us: Go! 2018 Summer](https://academy.realm.io/kr/posts/ios-fastlane-demos/)

[fastlane 공식문서](https://docs.fastlane.tools)

# fastlane이란?

[공식문서 소개] `fastlane`은 iOS 및 안드로이드 앱의 베타 버전 배포와 릴리즈를 자동화할 수 있는 가장 쉬운 방법입니다. 스크린샷을 만들고, 코드 사이닝*code signing*을 하고, 앱을 릴리즈하는 것과 같은 모든 따분한 작업들을 처리합니다.

# 설치하기

- 먼저 최신 Xcode 커맨드라인 툴을 설치한다.

  - `xcode-select --install`

- 이제 `fastlane`을 설치한다. 내 컴퓨터에서는 `gem install fastlane`이 안먹혀서 `brew`로 설치했다.

  - `brew cask install fastlane`

- zsh를 사용하고 있다면 `~/.zshrc` 파일에 다음을 추가해주자.

  ```shell
  export PATH="$HOME/.fastlane/bin:$PATH"
  export LC_ALL=en_US.UTF-8
  export LANG=en_US.UTF-8
  ```

- 터미널을 재실행한다.

# fastlane init

- `fastlane`을 사용할 프로젝트 디렉토리로 이동한다.
- `fastlane init`을 입력하여 `fastlane`을 초기화한다.

# Actions

액션과 관련해서 공식 문서를 참고하자. 설명이 잘 되어 있는 것 같고, 액션 하나에도 여러 옵션이 존재해 더욱 다채롭게 활용할 수 있다.

### cert

`fastlane cert`

- 인증서를 발급받는다.

### match

`fastlane match`

- 팀 간 인증서와 프로파일을 동기화한다. 개발 팀과 하나의 코드 사이닝 아이덴티티를 공유하여 코드 사이닝 이슈를 예방하고 단순화한다.

### sigh

`fastlane sigh`

- 프로비저닝 프로파일을 발급받는다.

### pem

`fastlane pem`

- 자동으로 푸시 노티피케이션 프로파일을 생성하고 갱신한다.
  - 새로운 사이닝 요청 생성
  - 새로운 푸시 인증서 생ㅇ성
  - 인증서 다운로드
  - 현재 디렉토리에 새로운 `.pem` 파일 생성

### gym

`fastlane gym`

- 빌드하여 ipa 파일을 만들어준다.
- `[프로젝트명].app.dSYM.zip` / `[프로젝트명].ipa` 파일이 생성된다.

### deliver

`fastlane deliver`

- 로컬에 있는 파일의 내용을 앱스토어 커넥트에 등록된 앱의 앱 정보에 업로드한다.
- `fastlane deliver init`을 먼저 실행한다.
  - 계정 및 인증번호를 입력해야 한다.
  - `Deliverfile` 파일, `metadata` / `screenshots` 폴더 등이 생성된다.
    - `metadata` 폴더에 있는 여러 파일을 수정하여 앱스토어 커넥트에 업로드할 수 있다.
    - 앱스토어 커넥트에 있는 모든 스크린샷 정보를 가져와 `screenshots` 폴더에 저장한다.
- `fastlane deliver`를 실행하여 앱스토어 커넥트에 정보를 업로드한다.
  - `--skip_screenshots` 옵션 : 스크린샷 관련 내용은 제외하고 업로드한다.

### snapshot

`fastlane snapshot`

- 스크린샷을 찍어서 저장해준다.

- `fastlane snapshot init`을 먼저 실행한다.

  - `SnapFile` 등의 파일이 생성됨. 기기 / 지역화 언어 등을 설정할 수 있다.
    - 초기에 모두 주석처리 되어 있으니 수정해주자.

- 내부적으로 UI Test 기능을 사용하여 스크린샷을 찍는 것

  1. 프로젝트에 새로운 UI Test 타겟 추가하기
  2. 생성된 `SnapshotHelper.swift` 파일을 전 단계에서 생성한 UI Test 타겟에 추가하기
  3. UI Test의 메인 파일에서 `XCUIApplication().launch()` 코드를 삭제하고 그 위치에 아래의 코드를 추가하기

  ```swift
  let app = XCUIApplication()
  setupSnapshot(app)
  app.launch()
  ```

  4. `testExample()`의 구현부 부분에 커서를 두고 하단의 레코딩 버튼을 누른다. 시뮬레이터가 실행되고 앱이 실행된다. 앱에서 UI가 변경될 때 코드가 자동으로 구현부 내에 한 줄씩 생성된다.
  5. 원하는 위치에 `snapshot(_:)` 함수를 호출한다. 인자로 스크린샷의 이름으로 사용될 문자열이 들어간다.
  6. `fastlane snapshot` 명령어를 입력한다.
  7.  `Snapshot` 파일로부터 저장한 기기 및 지역화 언어 정보를 가져오고 스크린샷을 찍어 저장한다.

- `UserDefaults.standard.bool(forKey: "FASTLANE_SNAPSHOT")` 플래그값을 이용하여 스냅샷을 찍을 때 동작하게 하는 코드를 설정해줄 수 있다.

- 시간이 적게 걸리진 않는 것 같다. 빌드도 다시 하고 기기마다 시뮬레이터도 다시 실행하여 촬영한다.

### frameit

`fastlane frameit`

- 저장한 스크린샷에 기기에 맞는 프레임을 씌운 이미지를 저장해준다.
- `fastlane frameit` / `fastlane frameit silver` / `fastlane frameit rosegold`

### produce

`fastlane produce`

- Xcode에서 프로젝트를 생성하지 않고 앱스토어 커넥트 및 개발자 포털에 새로운 앱을 생성한다.
- 계정 / 앱 식별자 / 앱 이름 등을 입력해준다.
- `-i` 옵션을 붙여 앱스토어 커넥트에는 앱이 생성되지 않게 할 수 있다.

### pilot

`fastlane pilot`

- 테스트플라이트 테스터와 빌드를 관리
- `fastlane pilot [명령어] -u [계정]`과 같이 입력하여 실행한다.
  - 명령어에는 upload / add / list / find 등이 있다.
- 테스트플라이트를 사용하게 되었을 때 다시 알아보자...

### 커스텀 액션

`fastlane [custom_lane]`

- `Fastfile`에 실행할 명령어(액션)를 입력해두어 여러 명령어를 순차적으로 실행할 수 있게 해준다.
- 다음의 위치에 명령어를 입력해둔다. 아래의 경우 `fastlane custom_lane`을 실행하여 동작하게 할 수 있다.

```shell
platform :ios do
	desc "Description of what the lane does"
	lane :custom_lane do
		increment_build_number		# 빌드 번호를 자동으로 하나 증가시킴.
		increment_version_number	# patch 버전을 하나 증가시킴. (1.0.0 -> 1.0.1)
		gym							# 빌드하여 ipa 파일 생성
		snapshot					# 스크린샷 저장
		frameit						# 저장한 스크린샷에 프레임 씌우기
		deliver						# 앱스토어 커넥트에 정보 업로드하기
	end
end
```

### badge

`fastlane badge`

- 앱 아이콘에 뱃지를 달아줌
- deprecated 되었으며 badge plugin을 대신 사용하기

---

cert, sigh, pem과 관련해서 잘 알아두면 협업시 도움이 많이 될 것 같다.

일단 관련 개념부터 익히자..