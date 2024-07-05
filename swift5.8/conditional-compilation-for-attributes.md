## Conditional compilation for attributes

아래는 컴파일러 버전이 5.6 이상일 때만 `@preconcurrency` 속성을 사용한다. 

```swift
#if compiler(>=5.6)
@preconcurrency protocol P: Sendable {
  func f()
  func g()
}
#else
protocol P: Sendable {
  func f()
  func g()
}
#endif
```

이는 전체 프로토콜을 복제해서 코드 중복이 발생하고, 일부 속성은 컴파일러 버전이 아니라 플랫폼 혹은 플래그에 따라 가용성이 달라지는 문제가 있다.

&nbsp;
## 솔루션

[SE-0367](https://github.com/apple/swift-evolution/blob/main/proposals/0367-conditional-attributes.md)에서 `#if`는 새 속성을 채택하기 위해 선언을 복제할 필요없이 속성을 둘러쌀 수 있게 하고, 컴파일러가 특성을 지원하는지 확인할 수 있는 `hasAttribute(AttributeName)` 조건부 지시문을 제안한다.

```swift
#if hasAttribute(preconcurrency)
@preconcurrency
#endif
protocol P: Sendable {
  func f()
  func g()
}
```

이는 프로토콜을 복제하지 않아서 코드 중복이 발생하지 않고 `hasAttribute` 조건문을 사용해서 가용성을 명확하게 확인한다.
