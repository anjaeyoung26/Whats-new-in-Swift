## Using if/else and switch statements as expressions

Swift 5.9부터 if/else 및 switch 문을 표현식으로 사용할 수 있다. Swift 5.9 이전에는 복잡한 조건으로 변수를 초기화하려면 3항식을 사용했다:

```swift
let bullet =
  isRoot && (count == 0 || !willExpand) ? ""
    : count == 0     ? "- "
    : maxDeptch <= 0 ? "▹ " : "▿ "
```

Swift 5.9의 if 표현식은 훨씬 친숙하게 만들어준다:

```swift
let bullet =
  if isRoot && (count == 0 || !willExpand) { "" }
  else if count == 0 { "- " }
  else if maxDepth <= 0 { "▹ " }
  else { "▿ " }
```

글로벌 변수나 저장 프로퍼티를 초기화할 때도 유용하다. 이전에는 초기화할 때 조건이 필요하다면 클로저를 사용했다:

```swift
let attributedName = {
  if let displayName, !displayName.isEmpty {
    AttributedString(markdown: displayName)
  } else {
    "Untitled"
  }
}

// Swift 5.9
let attributedName =
  if let displayName, !displayName.isEmpty {
    AttributedString(markdown: displayName)
  } else {
    "Untitled"
  }
```

Swift 5.9의 switch 표현식은 아래와 같이 사용할 수 있다:

```swift
let result = switch score {
  case 0...60: "Fail"
  case 61...70: "Good"
  case 71...80: "Great"
  case 81...100: "Excellent"
  default: "Unknown"
}
```

자세한 내용은 [What's new in Swift 2:44](https://developer.apple.com/videos/play/wwdc2023/10164/?time=164)에서 확인할 수 있다.
