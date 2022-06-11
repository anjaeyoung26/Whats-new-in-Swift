## if let shorthand

[SE-0345](https://github.com/apple/swift-evolution/blob/main/proposals/0345-if-let-shorthand.md)는 동일한 이름의 섀도우 변수로 옵셔널 언래핑하기 위한 새로운 단축 구문을 도입했다. 이는 다음과 같은 코드를 작성할 수 있음을 의미한다.

```swift
var name: String?

if let name {
  print("Hello, \(name)!")
}
```

이전에는 `if let`을 사용하여 옵셔널 언래핑을 하기 위해 다음과 같이 코드를 작성했다.

```swift
if let name = name {
  print("Hello, \(name)!")
}

if let unwrappedName = name {
  print("Hello, \(unwrappedName)!")
}
```

이 변경 사항은 객체 내부의 속성으로 확장되지 않으므로, 다음과 같은 코드는 작동하지 않을 것이다.

```swift
struct User {
  var name: String
}

let user: User? = User(name: "Linda")

if let user.name { // X
  print("Welcome, \(user.name)!")
}
```