# Stack

`import java.util.*;`

`Stack<T> stack = new Stack<>();`

- `push(T)` : 스택에 요소 삽입
- `pop()` : 스택에서 가장 위에 있는 요소 반환하고 없앰
- `peek()` : 스택에서 가장 위에 있는 요소 반환
- `isEmpty()` : 스택이 비어있는지를 반환
- `size()` : 스택에 있는 요소의 개수 반환

# Queue

`import java.util.*;`

`Queue<T> queue = new LinkedList<>();`

- add(T) : 큐에 요소 삽입
- peek() : 가장 먼저 큐에 들어간 요소 반환
- `remove()` : 가장 먼저 큐에 들어간 요소 반환하고 없앰
- `isEmpty()` : 큐가 비어있는지를 반환
- `size()` : 큐에 있는 요소의 개수 반환

# Deque

`import java.util.*;`

`Deque<T> deque = new LinkedList<>()`;

- `addFirst(T)` : 덱의 앞에 요소 삽입
- `addLast(T)` : 덱의 뒤에 요소 삽입
- `peekFirst()` : 덱의 앞에 있는 요소 반환
- `peekLast()` : 덱의 뒤에 있는 요소 반환
- `pollFirst()` : 덱의 앞에 있는 요소 반환하고 없앰
- `pollLast()` : 덱의 뒤에 있는 요소 반환하고 없앰

---

# BufferedReader

`import java.io.*`;

`public static void main(String[] args) throws IOException { ... }`

`BufferedReader in = new BufferedReader(new InputStreamReader(System.in));`

- `readLine()` : 다음 한 줄을 읽어들여 문자열 반환
- `Integer.parseInt(in.readLine());`과 같이 작성하여 읽어들인 문자열을 정수형으로 파싱 가능
- Scanner를 쓰는 것보다 빠른 입력이 가능함

# StringBuffer

`StringBuffer stringBuffer = new StringBuffer();`

- `append(String)` : 문자열 이어붙이기
- `deleteCharAt(Int)` : 특정 인덱스에 있는 문자열 요소 없애기
- `System.out.println()`은 오래 걸리고, `StringBuffer`를 활용하여 문자열을 모아 한꺼번에 출력하여 시간을 줄일 수 있음

# BigInteger

`BigInteger bigInteger = new BigInteger(String)`

- 여러 인스턴스 메소드 활용하여 사칙연산 수행 가능