## Type placeholders

[SE-0315](https://github.com/apple/swift-evolution/blob/main/proposals/0315-placeholder-types.md)는 type placeholders의 개념을 도입해서 값타입의 일부만 지정하고 나머지는 타입 유추를 사용할 수 있다. 아래와 같이 타입 대신에 _를 명시해서 Swift가 타입을 추론하도록 한다.

```swift
let score: _ = 5
```

이는 컴파일러가 타입의 일부만 추론할 수 있게 한다. 아래 타입을 명시하지 않은 dictionary를 보자:

```swift
var results1 = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```

Swift는 dictionary의 타입을 `[String: [Any]]`로 추론할 것이다. 다음과 같이 전체 타입을 명시할 수 있다:

```swift
var results2: [String: [Int]] = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```

이때 type placeholders를 사용해서 dictionary 키의 타입만 추론하도록 할 수 있다. 또한 `_?`를 사용해서 `Optional` 타입으로도 추론할 수 있다.

```swift
var results3: [_ : [Int]] = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```
