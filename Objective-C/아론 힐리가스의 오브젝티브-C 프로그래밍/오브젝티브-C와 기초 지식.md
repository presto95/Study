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

# 개발자용 도움말 문서

하던 대로 잘 참고하면서 보기. Objective-C를 공부할 때는 Objective-C 언어로 구성된 문서를 확인하는 것이 좋겠다.

# 첫 클래스

### 객체를 기술하는 방법

- 클래스에서 구현하는 메소드들 (인스턴스 메소드와 클래스 메소드 모두 포함)
- 클래스의 각 인스턴스마다 그 안에서 정의되는 인스턴스 변수들

작성할 클래스는 파일 두 곳에서 정의됨

- `[].h` : 헤더 파일 / 인터페이스 파일. 인스턴스 변수들과 인스턴스 메소드들의 **선언**이 위치함
- `[].m` : 구현 파일. 각 메소드가 **구현**되는 곳

**헤더 파일 작성**

- 인스턴스 변수들을 선언한 곳은 중괄호로 묶여 있음
- 중괄호 밖에 메소드들을 선언함

```objective-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN
// @interface 선언, NSObject 클래스를 상속받음
@interface Person: NSObject {
    float heightInMeters;
    int weightInKilos;
}
// 메소드 선언. 함수 이름, 함수 반환형, 매개변수 타입, 매개변수 이름의 위치 등 잘 살피기
// 콜론 전후 공백 필요 없음
- (void)setHeightInMeters:(float)h;
- (void)setWeightInKilos:(int)w;
- (float)bodyMassIndex;
// @end 선언
@end

NS_ASSUME_NONNULL_END
```

**구현 파일 작성**

```objective-c
// Person.h 헤더 파일 임포트
#import "Person.h"
// @implementation 선언
@implementation Person
// 메소드 구현
- (void)setHeightInMeters:(float)h {
    heightInMeters = h;
}
- (void)setWeightInKilos:(int)w {
    weightInKilos = w;
}
- (float)bodyMassIndex {
    return weightInKilos / (heightInMeters * weightInKilos);
}
// @end 선언
@end
```

main.m 파일에서 사용하려면 헤더 파일을 임포트해주어야 함. 프로그래머가 만든 것이므로 쌍따옴표 내에 명시

- `#import "Person.h"`

## 액세서 메소드

- 객체의 인스턴스 변수들을 누가 와서 건드리지 못하도록 최대한 보호하는 것이 객체지향적 사고방식임
  - Objective-C의 클래스에는 기본적으로 외부에서는 인스턴수 변수에 접근할 수 없는 것으로 보임
  - 인스턴스 변수들에 접근할 수 있는 것은 그 변수들을 소유한 객체 뿐이다.
- 외부에서 객체의 인스턴스 변수들에 접근하기 위한 메소드가 필요함
- 접근 메소드 (accessor method) = 설정 메소드 (setter method) + 접근 메소드 (getter method)
  - Java의 접근자에는 `get???`과 같이 네이밍 되지만, Objective-C나 Swift에서는 접근자에 get을 명시하지 않는 것이 일반적이다.
  - 인스턴스 변수를 읽는 메소드의 이름에는 그 인스턴스 변수의 이름을 그대로 사용한다.

## 점 표기법

```objective-c
// 아래 두 문장은 완벽히 동일한 코드
p = [x foo];
p = x.foo;
// 설정자(set???)을 점 표기법으로 사용하면 프로퍼티에 값을 할당하는 것처럼 자동으로 변환되는 듯
[x setfido:3];
x.fido = 3;
```

- 저자는 Objective-C에서의 '메세지를 보낸다'는 의미가 분명하지 않아 점 표기법을 추천하지 않는다.

## 프로퍼티

- 액세서 메소드가 단순하게 작성될 수 있도록 프로퍼티라는 방법을 제공한다.
  - Swift의 프로퍼티와 같은 개념이 아니므로 다르게 이해하자.
- **설정 메소드와 접근 메소드를 한 행에서 선언할 수 있다.**
  - 설정자 (set???)와 접근자 (???)가 자동으로 만들어진다.

```objective-c
// [].h에서 설정 메소드와 접근 메소드를 선언한 것을 아래와 같이 변경함
// @property 선언 + 타입 + 변수명
@property float heightInMeters;
@property int weightInKilos;
// [].m에서 각 @property 선언을 근거로 기본 액세서 메소드를 통합하라고 알려주면 됨
// 설정 메소드와 접근 메소드의 구현 코드를 삭제하고 아래와 같이 작성
// @synthesize 선언
@synthesize heightInMeters, weightInKilos;
```

헤더 파일 구현

```objective-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface Person: NSObject {
    float heightInMeters;
    int weightInKilos;
}

@property float heightInMeters;
@property int weightInKilos;
- (float)bodyMassIndex;

@end

NS_ASSUME_NONNULL_END
```

구현 파일 구현

```objective-c
#import "Person.h"

@implementation Person

@synthesize heightInMeters, weightInKilos;
- (float)bodyMassIndex {
    return weightInKilos / (heightInMeters * weightInKilos);
}

@end
```

main.m 파일에서 `weightInKilos` 변수에 접근 가능하고, `setWeightInKilos()` 메소드가 자동으로 만들어져 사용 가능하다.

## self

- 어떤 메소드 안에서도 암시적 지역 변수인 self에 접근할 수 있다.
  - 엑세서 메소드 없이는 인스턴스 변수를 읽지도 쓰지도 않는 사람들을 위해 사용될 수 있음..
- self는 메소드의 실행 주체가 되는 객체의 포인터다.
- 객체가 자신에게 메세지를 보내야 할 때 self가 사용된다.
- self를 인수로 전달하면 현재 객체가 어디에 있는지 다른 객체들이 알 수 있음

```objective-c
heightInKilos;	// 인스턴스 변수에 접근함
[self heightInKilos];	// 프로퍼티에 접근함
```

## 복합 파일 구성

- [].m 파일과 [].h 파일은 빌드 시 따로따로 컴파일되어 하나로 합쳐짐
  - [].h 파일은 헤더 파일일 뿐 실행할 수 있는 코드가 담긴 파일이 아님
- 라이브러리 코드를 링크하여 넣을 수 있음

# 상속