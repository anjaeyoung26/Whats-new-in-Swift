## Struct opaque result types

[SE-0328](https://github.com/apple/swift-evolution/blob/main/proposals/0328-structural-opaque-result-types.md)는 *opaque type*이 반환될 수 있는 곳의 범위를 넓힌다. 예를 들어, 이제는 한 번에 둘 이상의 *opaque type*을 반환할 수 있다.


```swift
func showUserDetails() -> (some Equatable, some Equatable) {
  (Text("Username"), Text("@twostraws"))
}
```

또한 *opaque type*을 생성하는 데 실패할 수 있는 함수를 구현할 수 있다.

```swift
func showUserDetails() -> (some Equatable)? {
  Text("Username")?
}
```