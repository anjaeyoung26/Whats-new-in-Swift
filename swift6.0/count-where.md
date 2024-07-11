## count(where:)

Swift 6.0 이전에는 배열에서 특정 조건을 만족하는 요소의 개수를 구하기 위해 filter + count 구문을 사용했다:


```swift
[1, 2, 3, -1, -2].filter({ $0 > 0 }).count // 3
```

[SE-0220](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0220-count-where.md)은 filter + count 구문을 단순화한 `count(where:)`을 제안한다.

```swift
[1, 2, 3, -1, -2].count(where: { $0 > 0}) // 3
```