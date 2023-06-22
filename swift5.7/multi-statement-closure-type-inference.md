## Multi-statement closure type inference

[SE-0326](https://github.com/apple/swift-evolution/blob/main/proposals/0326-extending-multi-statement-closure-inference.md)는 클로저에 대한 매개변수 및 타입 추론을 사용하는 Swift의 기능을 개선한다. 즉 명시적으로 입출력 타입을 지정해야 했던 많은 곳이 제거될 수 있음을 의미한다. 이전에 Swift는 사소하지 않은 모든 클로저를 개선하기 위해 고심해왔고, Swift 5.7 이상에서는 다음과 같은 코드를 작성할 수 있다.

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

Swift 5.7 이전에는 다음과 같이 반환 타입을 명시적으로 지정해야 했다.

```swift
let oldResults = scores.map { score -> String in 
  if score >= 85 {
    return "\(score)%: Pass"
  } else {
    return "\(score)%: Fail"
  }
}
```