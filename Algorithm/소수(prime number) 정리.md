# 특정 수의 소수 판별

특정 수 N의 소수 판별을 위해서는 **2부터 sqrt(N)까지 순회하면서 모든 경우에 대하여 나누어 떨어지지 않는 것을 확인**하면 된다.

```java
boolean isPrime(int number) {
    for(int i=2; i*i<=number; ++i) {
        if(number % i == 0) {
            return false;
        }
    }
    return true;
}
```

```swift
func isPrime(_ number: Int) -> Bool {
    for i in 2...Int(sqrt(Double(number))) {
        if number % i == 0 {
            return false
        }
    }
    return true
}
```

`for`문 조건식의 경우 `i <= Math.sqrt(number)`와 같이 작성하는 것보다 `i * i <= number`와 같이 적는 것이 좋다. `Math.sqrt()`는 실수를 반환하여 오차가 생길 수 있기 때문이다.

# 구간 내 자연수의 소수 판별

**에라토스테네스의 체** 알고리즘 사용. 다음의 공간이 필요하다.

- 소수를 저장할 빈 배열. 배열의 크기는 *구간의 끝 + 1*로 해준다.
- 소수의 개수를 저장할 변수. 이는 소수를 저장할 빈 배열의 인덱스로 활용 가능
- 수가 지워졌는지 판별하기 위한 boolean 타입 배열
  - 2부터 구간의 끝까지 순회하면서,
  - 해당 인덱스의 배열 값이 false이면 그 수는 소수이므로 소수를 저장할 빈 배열에 추가하고, 배열 값을 true로 설정하고, 그 배수를 지워나감

```java
int count = 0;
int[] array = new int[n + 1];
boolean[] isCleared = new boolean[n + 1];
for(int i=2; i<=n; ++i) {
    if(!isCleared[i]) {
        array[count++] = i;
        for(int j = i * 2; j<=n; j+=i) {
            isCleared[j] = true;
        }
    }
}
```

```swift
var primes = [Int]()
var isCleared = Array(repeating: false, count: n + 1)
for i in 2...n {
    if !isCleared[i] {
        primes.append(i)
        for j in stride(from: i * 2, through: n, by: i) {
            isCleared[j] = true
        }
    }
}
```

