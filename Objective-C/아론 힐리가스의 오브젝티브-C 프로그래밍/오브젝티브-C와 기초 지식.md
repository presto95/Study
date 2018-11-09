# 객체

메소드 : **메세지**로 트리거되는 함수

```objective-c
NSDate *now = [NSDate date];
NSLog(@"The new date lives at %@", now);
```

- date 메세지를 NSDate 클래스에 보냄. 
- date 메소드는 NSDate의 인스턴스를 만들고 현재 날짜 및 시간을 초기화한 후, 새 객체가 시작하는 곳의 주소를 리턴하도록 NSDate 클래스에 요청함. 
- 리턴받은 주소는 now 변수에 저장됨. 이 변수는 NSDate 객체를 가리키는 포인터.
- NSLog()는 C의 printf()와 비슷함. 형식 문자열의 시작이 @이고, 개행을 위해 \n을 작성할 필요가 없음
- NSLog()는 출력 결과 앞에 날자와 시간, 프로그램 이름, 프로세스 ID를 붙임

Swift로 컨버팅한 코드

```swift
let now = Date()
print("The new date lives at \(now)")
```

## 메세지 해부

메세지 보내기는 항상 대괄호로 묶이고, 크게 두 부분으로 구성됨

- 메세지를 받는 객체의 포인터
- 트리거될 메소드의 이름

```objective-c
double seconds = [now timeIntervalSince1970];
NSLog(@"It has been %f seconds since the start of 1970.", seconds);        
NSDate *later = [now dateByAddingTimeInterval:100000];
NSLog(@"In 100,000 seconds it will be %@", later);
```

`[now dateByAddingTimeInterval:100000]`에서,

- now : 리시버*receiver*. 메세지를 받을 객체의 주소
- dateByAddingTimeInterval : 셀렉터*selector*. 트리거할 메소드의 이름
- 100000 : 인수

Swift로 컨버팅한 코드

```swift
let seconds: Double = now.timeIntervalSince1970
print("It has been \(seconds) seconds since the start of 1970.")
let later = now.addingTimeInterval(100000)
print("In 100,000 seconds it will be \(later)")
```

## 메모리 속 객체

now와 later는 지역 변수이므로 스택에 존재하며, 각각이 가리키는 NSDate 인스턴스는 힙에 존재한다.

## id

**어떤 Objective-C 객체를 가리키는 포인터**

정확히 어떤 객체를 가리킬지 특정하지 않고 포인터만을 만들어야 할 때 id라는 타입을 사용할 수 있음

```objective-c
id delegate;
```

id가 애스터리스크를 내포하고 있음

# 메세지 심화

```objective-c
NSDate *date = [NSDate date];
```

- date 메소드는 클래스 메소드.
  - NSDate 클래스에 메세지를 보내어 메소드가 실행되도록 함
- date 메소드는 NSDate 인스턴스의 포인터를 리턴

```objective-c
double seconds = [now timeIntervalSince1970];
```

- timeIntervalSince1970 메소드는 인스턴스 메소드.
  - 클래스의 인스턴스에 메세지를 보내어 메소드가 실행되도록 함
- timeIntervalSince1970 메소드는 double을 리턴

```objective-c
NSDate *later = [now dateByAddingTimeInterval:100000];
```

- dateByAddingTimeInterval 메소드는 인스턴스 메소드.
- 이 메소드는 인수를 하나 받으며, 메소드 이름에 콜론을 붙여 나타냄
- NSDate 인스턴스의 포인터를 리턴

## 메세지 겹쳐 보내기

alloc이라는 클래스 메소드는 초기화해야하는 새 객체의 포인터를 리턴함. 새 객체에 init 메세지를 보낼 때 사용함

Objective-C에서 객체를 만들 때 가장 흔히 사용하는 방법이 alloc과 init임

```objective-c
[[NSDate alloc] init];
```

메세지 보내기를 겹쳐 작성한 것.

- alloc은 NSDate 클래스로 보내짐
- 그 결과 (새로 만든 인스턴스의 포인터)가 init을 받음

```objective-c
// 아래 두 문장은 같은 역할을 수행함
NSDate *date = [NSDate date];
NSDate *date = [[NSDate alloc] init];
```

## 여러 개의 인수

다음과 같이 작성할 수 있다.

```objective-c
NSUInteger day = [cal ordinalityOfUnit:NSCalendarUnitDay inUnit:NSCalendarUnitMonth forDate:now];
```

Swift로 컨버팅한 코드

```swift
let day: Int = cal.ordinality(of: .day, in: .month, for: now)
```

## nil에 메세지 보내기

Objective-C에서는 nil에 메세지를 보내도 무방함 : 아무 일도 하지 않음

- 메세지를 보내고 아무런 일도 일어나지 않게 할 때는 nil로 설정된 포인터에 메세지를 보내면 안 됨
- nil에 메세지를 보내면 아무런 의미가 없는 리턴 값을 받음

# NSString

NSString의 인스턴스들은 문자들로 구성된 열을 담음. 리터럴 인스턴스도 만들 수 있음

stringWithFormat 클래스 메소드를 사용하여 인스턴스 생성 가능

```objective-c
NSString *x = [NSString stringWithFormat:@"The best number is %d", 5];
```

length 인스턴스 메소드를 사용하여 문자열에 포함된 문자의 개수를 가져올 수 있음

```objective-c
NSUInteger count = [x length];
```

# NSArray

NSArray의 인스턴스는 다른 객체들을 가리키는 포인터 리스트를 담음