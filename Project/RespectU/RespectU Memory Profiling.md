# 시작

[Memory Management in Swift: Understanding Strong, Weak and Unowned References](https://www.appcoda.com/memory-management-swift/?utm_source=drip&utm_medium=email&utm_campaign=design-pattern-memory-management) 글을 읽으면서 ARC에 대한 개념을 다시 한번 살피고 있었는데, 글 말미에 메모리 누수 추적에 관한 문장이 짤막하게 나왔다.

> For example, if you want to detect memory leaks, you can go up to the menu and select Product > Profile > Leaks.

내 앱이 여기에 자유로운지 확인해보고 싶어졌다.



# 프로파일링 과정

앱 실행 과정은 다음과 같다.

1. 앱 실행 
   - 닉네임과 버전을 받아오는 네트워킹을 수행한다.
2. 가이드 부분으로 넘어가기
   - 내비게이션 컨트롤러를 이용해서 다음 화면으로 이동한다.
3. 음악 화면 띄우기
   - 앱 내의 데이터를 보여주는데, 모든 탭을 로드하게 했고, 가장 많은 데이터를 로드하게 된다.
4. 미션 화면 띄우기 / 트로피 화면 띄우기 / 도전과제 화면 띄우기
   - 앱 내의 데이터를 가져다가 뿌려준다.
5. 다운로드 화면 띄우기
   - 서버에서 성과 정보를 받아오는 네트워킹을 수행하고, 내부 데이터베이스를 갱신하는 작업을 수행한다.



# 프로파일링 전

시작 : 18.83MB ~ 22MB

최고 : 51MB

끝 : 31.79MB

최고 메모리 점유율은 음악 화면에서 모든 탭을 로드했을때 일어났다. 하지만 별다른 메모리 누수는 일어나지 않은 것 같다.

`CFNetwork` 클래스에서 엄청 잡아먹는다. 구글링을 해보았다. 

세션을 명시적으로 종료해주지 않아서 일어나는 문제라고 한다.

`finishTasksAndInvalidate` 메소드를 각 핸들러에 추가해 주었다.



# 프로파일링 후 

시작 : 13.16MB ~ 24MB

최고 : 43MB

끝 : 26.53MB

메모리 점유율을 약 5MB 감소시켰다!

메모리 누수가 일어나는 곳도 확연히 줄어 단 한군데밖에 나오지 않았다.



# 마무리

신기했다. Instruments를 다각도에서 활용해 보아야겠다는 생각이 들었다.



# 참고한 링크

[URLSSession 사용시 참고](http://kka7.tistory.com/56?category=904185)

[CFNetwork 에서의 XTubeManager 관련 메모리 누수](http://appmaid.tistory.com/33)