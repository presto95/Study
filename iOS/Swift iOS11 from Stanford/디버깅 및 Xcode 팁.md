# 디버깅 팁

- 브레이크 포인트 설정하기
- lldb 활용하기
  - `po` : print object. 오브젝트 정보 확인하기
  - `p` : print. 오브젝트가 가지고 있는 인스턴스 프로퍼티 등을 확인
- `step over` : 한줄 한줄 실행해 나가기
- `step into` : 메소드가 있으면 타고 들어가기

[참고링크1](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb)

[참고링크2](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel)

# Xcode 프로젝트 파일이 담고 있는 것

## Target - Build Settings vs. Target - Build Phases

### Build Settings

- 현재 사용하는 프로그래밍 언어의 버전 등

### Build Phases

- Compile Resources : 앱 내에서 사용하고 있는 모든 파일이 위치함
- Link Binary with Libraries : 링크할 라이브러리를 명시함

