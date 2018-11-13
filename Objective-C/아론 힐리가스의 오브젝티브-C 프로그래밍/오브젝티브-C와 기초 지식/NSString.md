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

