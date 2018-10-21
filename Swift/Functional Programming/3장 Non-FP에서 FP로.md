# FizzBuzz

함수형 프로그래밍은 순수 함수들의 조합으로 이루어진다.

```swift
var i = 1
while i <= 100 {
    if i % 3 == 0 && i % 5 == 0 {
        print("fizzbuzz")
    }
    else if i % 3 == 0 {
        print("fizz")
    }
    else if i % 5 == 0 {
        print("buzz")
    }
    else {
        print(i)
    }

    i += 1
}
///
let fizz = { $0 % 3 == 0 ? "fizz" : "" }
let buzz = { $0 % 5 == 0 ? "buzz" : "" }
let fizzbuzz: (Int) -> String = { i in { a, b in b.isEmpty ? a : b }("\(i)", fizz(i) + buzz(i)) }
func loop(min: Int, max: Int, do f: (Int) -> Void) {
    Array(min...max).forEach(f)
}
loop(min: 1, max: 100) { print(fizzbuzz($0)) }

```

# High-Low

함수형 프로그래밍에서는 사이드 이펙트가 없도록 프로그래밍하기 때문에 전역 변수가 없는 것이 특징이다.

프로그램 전체적으로 유지되는 값도 매개 변수를 통해 입력받고 함수 간에 전달하면서 사용한다.

프로그램이 수행되는 과정을 프로그래밍하는 것이 아니다.

**선언형 프로그래밍** : 어떠한 동작을 해야 하는지, 어떠한 결과가 나와야 하는지 정의하는 것

```swift
import Foundation

let answer = Int(arc4random() % 100) + 1
var count = 0
while true {
    let userInput = readLine()
    guard let unwrappedInput = userInput, let inputNumber = Int(unwrappedInput) else {
        print("Wrong")
        continue
    }
    if inputNumber == answer {
        print("Correct! : \(count)")
        break
    }
    if inputNumber > answer {
        print("High")
    }
    if inputNumber < answer {
        print("Low")
    }
    count += 1
}
///
enum Result: String {
    case wrong = "Wrong"
    case correct = "Correct!"
    case high = "High"
    case low = "Low"
}
func generateAnswer(_ min: Int, _ max: Int) -> Int {
    return Int.random(in: min...max)
}
func inputAndCheck(_ answer: Int) -> () -> Bool {
    return { printResult(evaluateInput(answer))}
}
func evaluateInput(_ answer: Int) -> Result {
    guard let inputNumber = Int(readLine() ?? "") else { return .wrong }
    if inputNumber > answer { return .high }
    if inputNumber < answer { return .low }
    return .correct
}
func printResult(_ result: Result) -> Bool {
    if case .correct = result { return false }
    print(result.rawValue)
    return true
}
func corrected(_ count: Int) {
    print("Correct! : \(count)")
}
func countingLoop(_ needsContinue: @escaping () -> Bool, _ finished: (Int) -> Void) {
    func counter(_ count: Int) -> Int {
        if !needsContinue() { return count }
        return counter(count + 1)
    }
    finished(counter(0))
}
countingLoop(inputAndCheck(generateAnswer(1, 100)), corrected)
```

# WeatherForecast

비동기적으로 발생하는 데이터는 함수를 통해 전달된다.

순수 함수는 항상 특정 입력에 대해 특정 출력을 내므로 동시에 수행되더라도 서로의 동작에 영향을 받지 않는다.

```swift
import Foundation

struct Location: Codable {
    let title: String
    let location_type: String
    let woeid: Int
    let latt_long: String
}
struct WeatherInfo: Codable {
    let consolidated_weather: [Weather]
}
struct Weather: Codable {
    let weather_state_name: String
    let wind_direction_compass: String
    let applicable_date: String
    let min_temp: Float
    let max_temp: Float
    let the_temp: Float
    let wind_speed: Float
    let wind_direction: Float
    let air_pressure: Float
    let humidity: Float
    let visibility: Float
    let predictability: Float
}
///
let query = "seoul"
let locQueryUrl = URL(string: "https://www.metaweather.com/api/location/search?query=\(query)")!
if let locData = try? Data(contentsOf: locQueryUrl) {
    if let locations = try? JSONDecoder().decode([Location].self, from: locData) {
        for location in locations {
            print("[\(location.title)]")
            let woeid = location.woeid
            let weatherUrl = URL(string: "https://www.metaweather.com/api/location/\(woeid)")!
            if let weatherData = try? Data(contentsOf: weatherUrl) {
                if let weatherInfo = try? JSONDecoder().decode(WeatherInfo.self, from: weatherData) {
                    let weathers = weatherInfo.consolidated_weather
                    for weather in weathers {
                        let state = weather.weather_state_name.padding(toLength: 15,                                                          withPad: " ", startingAt: 0)
                        let forecast = String(format: "%@: %@ %2.2f°C ~ %2.2f°C", weather.applicable_date, state, weather.max_temp, weather.min_temp)
                        print(forecast)
                    }
                }
            }
            print("")
        }
    }
}
///
func getData(_ url: URL, _ completed: @escaping (Data) -> Void) {
    DispatchQueue.global(qos: .default).async {
        if let data = try? Data(contentsOf: url) {
            completed(data)
        }
    }
}
func getLocation(_ query: String, _ completed: @escaping ([Location]) -> Void) {
    let locQueryUrl = URL(string: "https://www.metaweather.com/api/location/search?query=\(query)")!
    getData(locQueryUrl) { data in
        if let locations = try? JSONDecoder().decode([Location].self, from: data) {
            completed(locations)
        }
    }
}
func getWeathers(_ woeid: Int, _ completed: @escaping ([Weather]) -> Void) {
    let weatherUrl = URL(string: "https://www.metaweather.com/api/location/\(woeid)")!
    getData(weatherUrl) { data in
        if let weatherInfo = try? JSONDecoder().decode(WeatherInfo.self, from: data) {
            completed(weatherInfo.consolidated_weather)
        }
    }
}
func printWeather(_ weather: Weather) {
    let state = weather.weather_state_name.padding(toLength: 15, withPad: " ", startingAt: 0)
    let forecast = String(format: "%@: %@ %2.2f°C ~ %2.2f°C", weather.applicable_date, state, weather.max_temp, weather.min_temp)
    print(forecast)
}
getLocation("san") { locations in
    locations.forEach({ location in
        getWeathers(location.woeid, { weathers in
            print("[\(location.title)]")
            weathers.forEach({ weather in
                printWeather(weather)
            })
            print()
        })
    })
}
RunLoop.main.run()
```

