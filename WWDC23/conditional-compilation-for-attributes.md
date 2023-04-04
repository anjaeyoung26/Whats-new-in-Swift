## Conditional compilation for attributes

아래의 코드는 컴파일러 버전이 5.6 이상일 경우에만 `@preconcurrency` 속성을 사용한다. 

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

이 방법은 두 가지 이유로 만족스럽지 않다. 먼저, 컴파일러 버전에 따라 `@preconcurrency` 속성을 적용할지 여부를 결정하는데 전체 프로토콜을 복제해서 많은 코드 중복이 발생한다. 그리고 일부 속성은 컴파일러 버전이 아니라 플랫폼 및 구성 플래그에 따라 가용성이 달라진다. 

&nbsp;
## 솔루션

[SE-0367](https://github.com/apple/swift-evolution/blob/main/proposals/0367-conditional-attributes.md)에서 `#if`는 새 속성을 채택하기 위해 선언을 복제할 필요 없이 속성을 둘러쌀 수 있게 하고, 컴파일러가 특성을 지원하는지 확인할 수 있는 `hasAttribute(AttributeName)` 조건부 지시문을 추가하자고 제안한다.

```swift
#if hasAttribute(preconcurrency)
@preconcurrency
#endif
protocol P: Sendable {
  func f()
  func g()
}
```

프로토콜을 복제하지 않아서 코드 중복이 발생하지 않고, `hasAttribute` 조건문을 사용해서 가용성을 명확하게 확인한다.