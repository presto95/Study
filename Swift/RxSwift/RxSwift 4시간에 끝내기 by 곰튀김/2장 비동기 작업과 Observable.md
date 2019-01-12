# 2장 비동기 작업과 Observable

## 동기와 비동기

동기 : 어떠한 작업이 끝날 때까지 다른 작업을 못하게 막음

비동기 : 어떠한 작업이 실행되고 있는 것과 상관 없이 작업을 수행함

### DispatchQueue

- `sync` 와 `async`
- 병렬 큐와 직렬 큐
- 2 * 2 = 4가지 종류 (동기 병렬, 동기 직렬, 비동기 병렬, 비동기 직렬)

위와 같은 작업을 돕는 서드 파티 라이브러리가 다수 존재함

- PromiseKit / Bolt 등

### PromiseKit (라이브러리)

- JavaScript 등에서 사용되는 Promise의 개념을 구현함
- Promise 객체를 사용

### RxSwift (라이브러리)

- Observable 객체를 사용

---

비동기 작업을 위한 코드를 잘 작성하기 위해 Rx를 사용하는 것