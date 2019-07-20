# Adapter

### 어댑터 패턴

> 어댑터 패턴은 클라이언트가 요구하는 인터페이스를 지원하는 클래스로 "adaptee"를 래핑하여 다른 두 가지 호환되지 않는 타입 간의 연결을 제공하기 위해 사용된다.

클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환한다.

호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 연결될 수 있도록 한다.

### 코드

Old 인터페이스와 New 인터페이스가 있는데, Old 'adaptee'를 래핑하여 두 개의 인터페이스를 연결시켰다.

```swift
protocol NewDeathStarSuperLaserAiming {
  var angleV: Double { get }
  var angleH: Double { get }
}

/* Adaptee */

struct OldDeathStarSuperLaserTarget {
  let angleHorizontal: Float
  let angleVertical: Float
}

/* Adapter */

struct NewDeathStarSuperLaserTarget: NewDeathStarSuperLaserAiming {
  private let target: OldDeathStarSuperLaserTarget
  
  init(target: OldDeathStarSuperLaserTarget) {
    self.target = target
  }
  
  var angleV: Double {
    return Double(target.angleVertical)
  }
  var angleH: Double {
    return Double(target.angleHorizontal)
  }
}

/* 사용 */

let target = OldDeathStarSuperLaserTarget(angleHorizontal: 14, angleVertical: 12)
let newFormat = NewDeathStarSuperLaserTarget(target)

newFormat.angleH
newFormat.angleV
```