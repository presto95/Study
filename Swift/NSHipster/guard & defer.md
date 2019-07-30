# guard & defer

> "우리는 (지혜로운 프로그래머들이 우리의 한계를 인지하는 것처럼) 최선을 다해야 한다... (텍스트 공간에 산재된) 프로그램과 (시간에 산재된) 프로세스 간 조화가 가능한 한 사소하도록."

> 다익스트라, "Go To Considered Harmful"

그의 에세이가 프로그래머들과 그들의 잘못된 온라인 논쟁 사이에서 "XX는 해로운 것으로 여겨진다"의 밈으로 유명해져 가장 많이 기억된다는 것은 부끄러운 일입니다. 다익스트라는 (늘 그렇듯이) **코드 구조는 그 행위를 반영해야 한다**라는 훌륭한 포인트를 잡아냈기 때문입니다.

Swift 2.0에서 우리가 작성하는 프로그램을 단순하게 하고 간소하게 하는 것을 목적으로 한 새로운 제어문 두 개, `guard` 와 `defer` 가 도입되었습니다. `guard` 는 본질적으로 코드를 더 선형적으로 만드는 반면, `defer` 는 코드의 실행을 지연시키는 것으로 반대의 일을 수행합니다.

어떻게 이 새로운 제어문에 접근해야 할까요? `guard` 와 `defer` 는 어떻게 프로그램과 프로세스 간 일치성을 분명하게 할 수 있을까요?

`defer` 는 미루어두고*defer*, 먼저 `guard` 를 살펴봅시다.

## guard

`guard` 는 실행을 지속시키기 위해 `true` 로 평가되는 표현식을 필요로 하는 제어문입니다. 표현식이 `false` 이면 반드시 필요한 `else` 절이 대신 실행됩니다.

```swift
func sayHello(numberOfTimes: Int) {
    guard numberOfTimes > 0 else {
        return
    }

    for _ in 1...numberOfTimes {
        print("Hello!")
    }
}
```

`guard` 문에 있는 `else` 절은 함수를 빠져나가기 위해 `return` 을 사용하거나, 루프를 빠져나가기 위해 `continue` 나 `break` 를 사용하거나, `fatalError(_:file:line:)` 과 같은 `Never` 를 반환하는 함수를 사용하여 반드시 현재 스코프를 빠져나가야 합니다.

`gurad` 문은 옵셔널 바인딩과 조합되었을 때 가장 유용하게 사용됩니다. `guard` 문의 조건에서 새롭게 생성된 옵셔널 바인딩은 함수나 블록의 나머지 부분에서 사용 가능합니다.

옵셔날 바인딩이 `guard-let` 문과 `if-let` 문에서 어떻게 동작하는지 비교해 봅시다.

```swift
var name: String?

if let name = name {
    // name은 내부에선 옵셔널이 아님 (name: String)
}
// name은 바깥에서는 옵셔널임 (name: String?)


guard let name = name else {
    return
}

// 이제부터 name은 옵셔날이 아님 (name: String)
```

Swift 1.2에서 도입된 다중 옵셔널 바인딩 구문이 깊은 들여쓰기 수준*renovation of the pyramid of doom*을 초래한다면, `guard` 문이 이를 완전히 허물 것입니다.

```swift
for imageName in imageNamesList {
    guard let image = UIImage(named: imageName)
        else { continue }

    // image로 어떠한 작업 수행
}
```

### Guarding Against Excessive Indentation and Errors

`guard` 가 코드를 어떻게 향상시키고 에러가 발생하는 것을 방지하는 것을 돕는지 전후를 살펴봅시다.

예제로 `readBedtimeStory()` 함수를 구현할 것입니다.

```swift
enum StoryError: Error {
    case missing
    case illegible
    case tooScary
}

func readBedtimeStory() throws {
    if let url = Bundle.main.url(forResource: "book",
                               withExtension: "txt")
    {
        if let data = try? Data(contentsOf: url),
            let story = String(data: data, encoding: .utf8)
        {
            if story.contains("👹") {
                throw StoryError.tooScary
            } else {
                print("Once upon a time... \(story)")
            }
        } else {
            throw StoryError.illegible
        }
    } else {
        throw StoryError.missing
    }
}
```

bedtime story를 읽기 위해 우리는 책을 찾을 수 있어야 합니다. 이야기책은 복호화 가능해야 하며, 이야기는 너무 무서워서는 안됩니다. (*이 책의 끝에는 괴물이 없어요. 아무쪼록 고마워요!*)

하지만 `throw` 문이 그 자신을 검증하는 것으로부터 얼마나 멀리 떨어져 있는지에 주목하시요. `book.txt` 를 찾지 못했을 때 어떤 일이 일어나는지 알아내려면 메소드의 아래까지 모든 부분을 읽어야 합니다.

좋은 책처럼, 코드는 이야기를 할 수 있어야 합니다. 따라가기 쉬운 구성과, 명료한 시작, 중간, 끝과 함께 말이죠. ("포스트모더니즘 장르"에 너무 많은 코드를 작성하려고만 하지 않으면 됩니다.)

`guard` 문의 전략적 사용은 코드를 더욱 선형적으로 읽을 수 있게 조직하도록 해줍니다.

```swift
func readBedtimeStory() throws {
    guard let url = Bundle.main.url(forResource: "book",
                                  withExtension: "txt")
    else {
        throw StoryError.missing
    }

    guard let data = try? Data(contentsOf: url),
        let story = String(data: data, encoding: .utf8)
    else {
        throw StoryError.illegible
    }

    if story.contains("👹") {
        throw StoryError.tooScary
    }

    print("Once upon a time... \(story)")
}
```

*훨씬 낫네요!* 각각의 에러 케이스는 확인되자마자 처리되므로, 왼쪽 방향에서 실행 흐름을 똑바로 따라갈 수 있습니다.

### Don't Not Guard Against Double Negatives

당신이 이 새로운 제어 흐름 매커니즘을 받아드릴 때 주의해야 할 습관은, 특히 평가된 조건이 이미 부인되었을 때 이를 과도하게 사용하는 것입니다.

예를 들어 문자열이 비어 있을 때 먼저 반환하기 원한다면, 다음과 같이 작성하지 마세요.

```swift
// Huh?
guard !string.isEmpty else {
  return
}
```

단순하게 하세요. (제어) 흐름을 생각하세요. 이중 부정을 피하세요.

```swift
// Aha!
if string.isEmpty {
  return
}
```

## defer

`guard` 와 에러 처리를 위한 새로운 `throw` 문 사이에서, Swift는 중첩 `if` 문보다 이른 반환 스타일을 장려합니다. 그러나 초기화된 (그리고 여전히 사용되는) 리소스가 함수 반환 이전에 정리되어야 할 대, 이른 반환은 별개의 도전거리를 일으킵니다.

`defer` 키워드는 실행이 현재 스코프를 벗어났을 때에만 실행될 블록을 선언하는 것으로 이 도전을 처리하기 위한 안전하고 쉬운 방법을 제공합니다.

시스템의 현재 호스트 이름을 반환하기 위해 `gethostname(2)` 시스템 콜을 감싼 다음의 함수를 생각해 봅시다.

```swift
import Darwin

func currentHostName() -> String {
    let capacity = Int(NI_MAXHOST)
    let buffer = UnsafeMutablePointer<Int8>.allocate(capacity: capacity)

    guard gethostname(buffer, capacity) == 0 else {
        buffer.deallocate()
        return "localhost"
    }

    let hostname = String(cString: buffer)
    buffer.deallocate()

    return hostname
}
```

여기서 우리는 일찍이 `UnsafeMutablePointer<Int8>` 을 할당했으나 실패 조건 *그리고* 버퍼로 어떠한 작업을 마쳤을 때 할당 해제해야 한다는 것을 기억할 필요가 있습니다.

에러가 발생하기 쉽나요? *그렇습니다.* 실망스럽게 반복적인가요? *그렇습니다.*

`defer` 문을 사용하면 프로그래머 에러의 가능성을 제거하고 코드를 단순하게 할 수 있습니다.

```swift
func currentHostName() -> String {
    let capacity = Int(NI_MAXHOST)
    let buffer = UnsafeMutablePointer<Int8>.allocate(capacity: capacity)
    defer { buffer.deallocate() }

    guard gethostname(buffer, capacity) == 0 else {
        return "localhost"
    }

    return String(cString: buffer)
}
```

`defer` 가 `allocate(capacity)` 호출 이후 즉시 온다고 할지라도, 그 실행은 현재 스코프의 끝까지 지연됩니다. `defer` 덕분에 `buffer` 는 함수가 어디에서 반환하든지간에 적절하게 할당 해제될 것입니다.

API가 `allocate(capacity:)` / `deallocate()` , `wait()` / `signal()` , `open()` / `close()` 와 같은 대칭 호출을 필요로 할 때마다 `defer` 를 사용하는 것을 고려해 보세요. 이러한 방법으로 프로그래머 에러의 잠재적 근원을 제거할 뿐만 아니라, 다익스트라의 자랑이 될 것입니다. *"간다!" 그는 자신의 모국어로 말할 것입니다.*

## Deferring Frequently

같은 스코프에서 여러 개의 `defer` 문을 사용한다면, 보이는 것에서 반대 순서로 실행됩니다. 스택처럼 말이죠. 이 반대 순서는 중요한 디테일인데, 지연되는 블록이 만들어질 대 스코프에 있는 모든 것들이 블록이 실행될 때 여전히 스코프에 있다는 것을 보장합니다.

예를 들어 다음의 코드는 아래와 같이 출력합니다.

```swift
func procrastinate() {
    defer { print("wash the dishes") }
    defer { print("take out the recycling") }
    defer { print("clean the refrigerator") }

    print("play videogames")
}
```

```text
play videogames

clean the refrigerator

take out the recycling

wash the dishes
```

> 이처럼 `defer` 문을 중첩시키면 어떻게 될까요?

```swift
defer { defer { print("clean the gutter") } }
```

> 스택의 맨 아래에 실행문을 push할 것이라고 먼저 생각했을 것입니다. 하지만 그런 일은 발생하지 않습니다. 이를 통해 생각해보고, 플레이그라운드에서 가설을 테스트해 보세요.

## Deferring Judgement

변수가 `defer` 문의 구현부에서 참조되면, 그것의 최종 값이 평가됩니다. 말하자면 `defer` 블록은 변수의 현재 값을 획득하지 않습니다.

```swift
func flipFlop() {
    var position = "It's pronounced /ɡɪf/"
    defer { print(position) }

    position = "It's pronounced /dʒɪf/"
    defer { print(position) }
}
```

```text
It's pronounced /dʒɪf/ 

It's pronounced /dʒɪf/        
```

## Deferring Demurely

명심해야 할 다른 것은 `defer` 블록이 그 스코프를 빠져나갈 없다는 것입니다. 그러므로 에러를 던질 수 있는 메소드를 호출하려 한다면, 에러는 주변 컨텍스트에 전달될 수 없습니다.

```swift
func burnAfterReading(file url: URL) throws {
    defer { try FileManager.default.removeItem(at: url) }
    // 🛑 에러 처리되지 않음

    let string = try String(contentsOf: url)
}
```

대신 `try?` 를 사용하여 에러를 무시하거나, 하던 것처럼 `defer` 블록 바깥으로 실행문을 옮기고 함수의 끝에서 실행하세요.

## (Any Ohter) Defer Considered Harmful

`defer` 문은 편리하지만, 그 능력이 혼란스럽고 추적하기 힘든 코드로 이끌 수 있다는 것을 인지하세요. 전형적인 `++` 후위 연산자의 구현에서처럼, 수정될 필요가 있는 값을 반환하는 함수에서 `defer` 를 사용하려는 유혹을 받게 될 것입니다.

```swift
postfix func ++(inout x: Int) -> Int {
    let current = x
    x += 1
    return current
}
```

이 경우 `defer` 는 현명한 대안을 제공해 줍니다. 증가를 지연시키기만 하면 되는데 왜 임시 변수를 생성해야 하나요?

```swift
postfix func ++(inout x: Int) -> Int {
    defer { x += 1 }
    return x
}
```

정말 현명하지만, 그럼에도 불구하고 함수 흐름의 역전은 가독성을 해칩니다. 할당된 리소스를 정리하는 대신 프로그램의 흐름을 명시적으로 변경하기 위해 `defer` 를 사용하면 실행 프로세스가 얽히고설키는 결과를 가져올 것입니다.

"현명한 프로그래머들이 우리의 한계를 인지하는 것처럼", 우리는 각 언어의 기능의 이득을 그 비용과 재어 보아야 합니다.

`guard` 와 같은 새로운 구문은 더 선형적이고 가독성이 좋은 프로그램으로 이끕니다. 가능하다면 많이 사용하세요.

이처럼 `defer` 는 중요한 도전을 해결해 주지만 그것이 우리의 시야 바깥으로 스크롤될 때 그 선언부를 추적하도록 강제합니다. 혼란과 불명료함을 막기 위해 최소한의 의도된 목적으로 그것을 사용하세요.