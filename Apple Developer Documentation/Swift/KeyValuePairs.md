# KeyValuePairs

**제네릭 구조체** | 키-값 쌍을 갖는 가벼운 컬렉션

---

## 정의

```swift
struct KeyValuePairs<Key, Value>
```

## 개요

키-값 쌍을 갖는 순서 있는 컬렉션이 필요하며, `Dictionary` 타입이 제공하는 빠른 키 색인 기능을 필요로 하지 않을 때 `KeyValuePairs` 인스턴스를 사용하라. 딕셔너리의 키-값 쌍과는 다르게, `KeyValuePairs`의 키나 값은 `Hashable` 프로토콜을 준수하지 않아야 한다.

---

딕셔너리와 비슷하게 키-값 쌍을 갖는 자료구조이지만, 키나 값은 `Hashable` 프로토콜을 준수하지 않고, 순서가 존재한다. 키는 중복될 수 있다.

특정 키와 일치하는 값을 찾는 등의 연산은 딕셔너리보다 느리며, 이 경우 컬렉션의 모든 요소를 검색해야 한다.

`ExpressibleByDictionaryLiteral` 프로토콜을 준수하여 딕셔너리 리터럴로 인스턴스를 생성하거나 매개변수로 넘겨줄 수 있지만, 타입을 명시할 때는 `KeyValuePairs<A, B>`와 같이 작성해야 한다.

내부적으로 `KeyValuePairs.Element`가 `(key: Key, value: Value)`의 타입 별칭으로 구현되어 있다.

---

### 딕셔너리 대신 KeyValuePairs를 사용해야 하는 경우

> Use a `KeyValuePairs` instance when you need **an ordered collection** of key-value pairs and **don't require the fast key lookup** that the `Dictionary` type provides.

- 순서 있는 키-값 쌍 컬렉션을 필요로 할 때
- 빠른 키 색인을 필요로 하지 않을 때