# 8장 Scheduler

## observeOn

`Scheduler` 를 인자로 받아 쓰레드 이동. `Observable` 반환

- `.observeOn(_:)` : `Scheduler` 를 인자로 받음
  - `MainScheduler.instance` : 메인 쓰레드 (`DispatchQueue.main`)
  - `ConcurrentDispatchQueueScheduler` : 병렬 쓰레드 (`DispatchQueue.global(qos:)`)

## subscribeOn

`Scheduler` 를 인자를 받아 쓰레드 이동.

subscribe 가 호출되어야 스트림이 동작하는 것이므로, 코드 시작 부분부터 영향을 미치게 됨

---

# Side-Effect

외부에 영향을 주는 것

사이드이펙트를 주어도 괜찮은 곳

- `subscribe` 메소드 구현부
- `do` 메소드 구현부
  - `do(onNext:onError:onCompleted:onSubscribe:onSubscribed:onDispose)`
  - 구독하는 것은 아님. 스트림 도중에 어떠한 작업을 수행하고 싶을 때 사용 가능