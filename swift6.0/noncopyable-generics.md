## Noncopyable Generics

[SE-0427](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0427-noncopyable-generics.md)는 Swift 6.0의 모든 struct, class, enum, protocol, generic type parameter는 `~Copyable`을 명시적으로 사용하지 않는 한 자동으로 `Copyable` 프로토콜을 채택한다. 그래서 copyable을 `Optional` 및 generic과 함께 사용할 수 없는 문제가 개선됐다.

```swift
struct FileDescriptor: ~Copyable {
  // Swift 5.9 error: cannot form a Optional<FileDescriptor>
  // Swift 6.0 O
  init?(fileName: String) { ... }
}
```