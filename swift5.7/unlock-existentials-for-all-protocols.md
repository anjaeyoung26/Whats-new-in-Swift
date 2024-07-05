## Unlock existentials for all protocols

[SE-0309](https://github.com/apple/swift-evolution/blob/main/proposals/0309-unlock-existential-types-for-all-protcols.md)는 `Self` 또는 `associatedtype`의 요구 사항이 있는 프로토콜을 타입으로 사용할 수 있다. 예를 들어, `Equtable` 프로토콜을 살펴보자:

```swift
public protocol Equatable {
  static func == (lhs: Self, rhs: Self) -> Bool
}
```

[`Equatable`](https://github.com/swiftlang/swift/blob/f8141e27b1b4240969c7b59b3c2cb6534483de7d/stdlib/public/core/Equatable.swift#L167)은 `Self` 요구 사항이 있는 프로토콜로, 이를 채택하는 타입을 참조한다. Swift 5.7 이전에는 `Equtable`과 같이 `Self` 요구 사항이 있는 프로토콜은 다음과 같이 사용할 수 없었다:

```swift
let tvShow: [any Equatable] = ["Brooklyn", 99]

for item in tvShow {
  if let item = item as? String {
    print("Found string: \(item)")
  } else if let item = item as? Int {
    print("Found integer: \(item)")
  }
}
```

왜냐하면 `Equtable`의 `==`가 동작하려면 동일한 타입의 두 인스턴스가 있어야 하고 `any Equtable`로 구체적인 타입을 숨기고 있기 때문이다. 하지만 Swift 5.7부터는 허용된다.

```swift
let firstName: any Equatable = "Paul"
let lastName: any Equatable = "Hudson"

if let firstName = firstName as? String, let lastName = lastName as? String {
  print(firstName == lastName)
}
```

또한 `Sequence`의 모든 요소가 `Identifiable` 프로토콜을 준수하는지 확인할 수 있다:

```swift
func canBeIdentified(_ input: any Sequence) -> Bool {
  input.allSatisfy { $0 is any Identifiable }
}
```
