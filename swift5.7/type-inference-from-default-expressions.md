## Type inference from default expressions

[SE-0347](https://github.com/apple/swift-evolution/blob/main/proposals/0347-type-inference-from-default-exprs.md)는 generic 타입의 매개변수에 기본값을 사용하는 기능을 확장한다. 다음과 같이 `Sequence`에서 무작위 원소의 개수를 반환하는 함수가 있다:

```swift
func drawLotto1<T: Sequence>(from options: T, count: Int = 7) -> [T.Element] {
  Array(options.shuffled().prefix(count))
}

print(drawLotto1(from: 1...49))
print(drawLotto1(from: ["Jenny", "Trixie", "Cynthia"], count: 2))
```

SE-0347은 `T` 매개변수의 기본값으로 구체적인 타입을 제공할 수 있게 한다:

```swift
func drawLotto2<T: Sequence>(from options: T = 1...49, count: Int = 7) -> [T.Element] {
  Array(options.shuffled().prefix(count))
}

print(drawLotto2(from: ["Jenny", "Trixie", "Cynthia"], count: 2))
print(drawLotto2())
```
