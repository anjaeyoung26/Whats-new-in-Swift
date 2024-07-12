## Partial Consumption

[SE-0429](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0429-partial-consumption.md)는 [noncopyable](../swift5.9/noncopyable-structs-and-enums.md) 타입을 부분적으로 consume 할 수 있도록 제안한다. 예를 들어, Swift 5.9는 아래의 코드에서 컴파일 에러가 발생한다:

```swift
struct Unique: ~Copyable {...}
struct Pair: ~Copyable {
  let first: Unique
  let second: Unique
}

extension Pair {
  consuming func swap() -> Pair {
    return Pair(
      first: second, // error: cannot partially consume 'self'
      second: first // error: cannot partially consume 'self'
    )
  }
}
```

`swap` 함수는 또 다른 `Pair`를 반환한 뒤 `deinit`을 호출한다. 하지만 `Pair` 타입에는 `first`, `second` 필드를 deinit하는 동작이 정의되어 있지 않다. 따라서 `swap` 함수가 종료되고 `Pair`는 자신을 consume 할 수 없다. 하지만 Swift 6.0은 deinit-less 집합체(`Pair`)에 있는 noncopyable 필드(`first`, `second`)가 개별적으로 consume 되는 것을 허용해서 `swap` 함수에서 컴파일 에러가 발생하지 않는다. 이는 deinit-less에만 허용하기 때문에 `Pair`의 deinit을 정의한다면 더 이상 유효하지 않다.