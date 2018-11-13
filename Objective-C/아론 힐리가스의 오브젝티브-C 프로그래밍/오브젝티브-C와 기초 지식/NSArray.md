# NSArray

NSArray의 인스턴스는 다른 객체들을 가리키는 포인터 리스트를 담음

```objective-c
// NSArray 초기화. 리스트의 끝에 nil이 들어감
NSArray *dateList = [NSArray arrayWithObjects:now, tomorrow, yesterday, nil];
```

C언어에서 사용하는 for문을 사용하여 배열 순차 탐색 가능

```objective-c
for (int i = 0; i < 3; ++i) { ... }
// 빠른 열거 사용법. Java와 비슷하게 생겼으나 콜론 대신 in을 사용함
for (NSDate *d in dateList) { ... }
```

## NSMutableArray

- NSArray의 인스턴스는 포인터들의 리스트로 만들어지며, 이 배열에는 포인터를 추가하거나 삭제할 수 없음
- NSMutableArray를 사용하여 포인터를 추가하거나 삭제할 수 있음. NSMutableArray는 NSArray의 서브클래스

```objective-c
// NSMutableArray의 array 클래스 메소드를 사용하여 빈 배열 만듦
NSMutableArray *dateList = [NSMutableArray array];
// addObject 인스턴스 메소드로 배열에 포인터 추가
[dateList addObject:now];
[dateList addObject:tomorrow];
// insertObject 인스턴스 메소드로 특정 인덱스에 포인터 추가
[dateList insertObject:yesterday atIndex:0];
// removeObject 인스턴스 메소드로 특정 인덱스의 포인터 제거
[dateList removeObject:0];
```



