## Noncopyable Switch

Swift 5.9에서 추가된 [noncopyable](../swift5.9/noncopyable-structs-and-enums.md) 타입의 `switch` 피연산자는 `consume`과 함께 사용해야 합니다.

```swift
enum Lunch: ~Copyable {
  case soup
  case salad
  case sandwich
}

let lunch = Lunch.soup

switch consume lunch {
case .soup:
  break
case .salad:
  break
case .sandwich:
  break
}

use(lunch) // error: lunch consumed by `switch`

// `if let` 혹은 `if case`도 동일하지만 본문에서는 `switch` 구문과 관련된 개선 사항으로 제외합니다.
let x: Optional = getValue()
if let y = consume x { ... }
use(y) // error: x consumed by `if let`
```

이는 noncopyable enum의 표현력을 심각하게 제한하므로, [SE-0432](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0432-noncopyable-switch.md)는 noncopyable의 패턴 매치가 consume해야 한다는 제한을 해제하고 case 블록에서 패턴을 매칭하는 동안 소유권을 가져오는(`borrowing`) 동작을 하도록 제안합니다. 따라서 noncopyable enum의 값을 consume하지 않고 `switch`와 패턴 매칭을 할 수 있습니다:

```swift
func isSoup(_ lunch: borrowing Lunch) -> Bool {
  switch lunch {
    case .soup: true
    default: false
  }
}
```