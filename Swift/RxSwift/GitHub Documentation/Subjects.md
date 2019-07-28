# Subjects

[여기]()에서 묘사된 것과 모두 정확히 같습니다.

# Relays

RxRelay는 두 가지 종류의 relay, `PublishRelay` 와 `BehaviorRelay` 를 제공합니다. 그것들은 정확히 대응하는 `Subject` 와 같이 동작하지만, 두 가지 변화가 있습니다.

- Relay는 절대 완료되지 않습니다.
- Rleay는 절대 에러를 배출하지 않습니다.

본질적으로, Relay는 오직 `.next` 이벤트만 배출하며, 절대 종료되지 않습니다.