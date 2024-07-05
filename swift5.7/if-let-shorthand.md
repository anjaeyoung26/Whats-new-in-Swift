## if let shorthand

[SE-0345](https://github.com/apple/swift-evolution/blob/main/proposals/0345-if-let-shorthand.md)는 동일한 이름으로 옵셔널 언래핑하기 위한 새로운 단축문을 도입했다.

```swift
var name: String?

/// Swift 5.7 ~
if let name {
  print("Hello, \(name)!")
}
```

단축문은 객체 내부의 속성으로 확장되지 않아서 다음과 같은 코드는 작동하지 않을 것이다.

```swift
struct User {
  var name: String
}

let user: User? = User(name: "Linda")

if let user.name { // X
  print("Welcome, \(user.name)!")
}
```
