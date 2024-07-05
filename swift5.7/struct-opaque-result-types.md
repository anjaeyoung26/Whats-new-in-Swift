## Struct opaque result types

[SE-0328](https://github.com/apple/swift-evolution/blob/main/proposals/0328-structural-opaque-result-types.md)는 opaque type이 반환될 수 있는 범위를 넓혀서 둘 이상의 opaque type을 반환할 수 있다:

```swift
func showUserDetails() -> (some Equatable, some Equatable) {
  (Text("Username"), Text("@twostraws"))
}
```

또한 opaque type을 생성하는데 실패할 수 있는 Optional 타입으로도 반환할 수 있다:

```swift
func showUserDetails() -> (some Equatable)? {
  Text("Username")?
}
```
