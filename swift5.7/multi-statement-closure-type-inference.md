## Multi-statement closure type inference

[SE-0326](https://github.com/apple/swift-evolution/blob/main/proposals/0326-extending-multi-statement-closure-inference.md)는 클로저에 대한 매개변수 및 타입 추론 기능을 개선한다. 이제 클로저의 반환 타입을 명시하지 않고 다음과 같은 코드를 작성할 수 있다:

```swift
let scores = [100, 80, 85]

let results = scores.map { score in 
  if score >= 85 {
    return "\(score)%: Pass"
  } else {
    return "\(score)%: Fail"
  }
}
```

이전에는 다음과 같이 반환 타입을 명시해야 했다.

```swift
let oldResults = scores.map { score -> String in 
  if score >= 85 {
    return "\(score)%: Pass"
  } else {
    return "\(score)%: Fail"
  }
}
```
