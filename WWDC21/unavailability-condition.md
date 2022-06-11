## Unavailability condition

[SE-0290](https://github.com/appl/swift-evolution/blob/main/proposals/0290-negative-availability.md)는 `#available`의 반전된 형태인 `#unavailable`을 도입했으며, 이는 가용성 확인이 실패할 경우 일부 코드를 실행한다. 이것은 `#available`을 빈 `true` 블록과 함께 사용하여, 특정 운영체제에서 사용할 수 없는 경우에만 실행해야 하는 코드가 있을 때 유용할 것이다. 따라서 다음과 같은 코드를 작성하는 것보다:

```swift
if #available(iOS 15, *) {} else {
  // Code to make iOS 14 and earlier work correctlry
}
```

`#unavailable`을 사용하여 다음과 같은 코드를 작성할 수 있다:

```swift
if #unavailable(iOS 15) {
  // Code to make iOS 14 and earlier work correctly
}
```

`#available`과 `#unavailable`의 또 다른 차이점은 플랫폼 와일드카드인 `*`이다. 이것은 `#available`과 함께 잠재적인 미래의 플랫폼을 허용하기 위해 필요하다. 즉, `if #available(iOS 15, *)`을 작성하면 일부 코드가 iOS 15 버전 이상 또는 기타 모든 플랫폼(macOS, tvOS, watchOS 및 미래의 알려지지 않은 플랫폼)에서 사용 가능한 것으로 표시된다.

하지만 이 동작은 `#unavailable`을 사용하면 더 이상 의미가 없다. `#unavailable(iOS 15, *)`는 *"다음 코드는 iOS 14 및 이전 버전에서 실행되어야 한다."*를 의미하지만, iOS 15를 사용할 수 없는 macOS, tvOS, watchOS, Linuex 등도 포함되어야 할까? 이러한 모호성을 피하기 위해 플랫폼 와일드카드(`*`)는 `#unavailable`과 함께 사용하지 못한다. 구체적으로 나열한 플랫폼만 테스트 대상으로 고려된다.

