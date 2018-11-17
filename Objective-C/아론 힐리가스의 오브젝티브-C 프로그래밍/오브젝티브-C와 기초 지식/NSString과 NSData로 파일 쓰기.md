# NSString과 NSData로 파일 쓰기

## NSString 인스턴스를 파일에 쓰기

`NSString`의 `writeToFile:atomically:encoding:error:` 인스턴스 메소드 사용하기

- 문자열 인코딩 : 문자가 바이트 배열에 저장되는 방식
- 대부분 UTF-8 사용

## NSError

파일 입출력에는 여러 종류의 문제가 발생하기 마련이다.

- 파일이 들어갈 디렉토리의 쓰기 권한이 없음
- 디렉토리 자체가 존재하지 않음
- 파일 시스템에 여유 공간이 없음
- 작업 마무리가 불가능한 상황
- 등등

작업의 성공 여부를 나타내는 부울 값 / **에러에 대한 설명을 메소드에서 반환하는 방법**이 필요하다.

- `writeToFile:atomically:encoding:error:` 인스턴스 메소드의 `error` 매개변수에 NSError 타입의 변수를 참조에 의한 전달로 넘겨줌
  - NSError의 localizedDescription 변수에 접근하여 에러 내용에 접근할 수 있음

```objective-c
// NSError 타입의 인스턴스를 생성하지 않고 포인터에 nil을 할당함
NSError *error = nil;
// NSError 타입 포인터 변수인 error의 포인터를 전달함. 이중 포인터
BOOL isSuccess = [str writeToFile:@"/tmp/cool.txt" atomically:YES encoding:NSUTF8StringEncoding error:&error];
```

## NSString으로 파일 읽기

`NSString`의 `initWithContentsOfFile:encoding:error:` 인스턴스 메소드 사용하기

```objective-c
NSError *error = nil;
NSString *str = [[NSString alloc] initWithContentsOfFile:@"/etc/resolv.conf" encoding:NSASCIIStringEncoding error:&error];
```

## NSData 객체를 파일에 쓰기 / 파일 읽기

NSData : 몇 바이트의 버퍼. 비트 가방

문서 보고 적절한 메소드 사용하기