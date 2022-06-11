## Type inference from default expressions

[SE-0347](https://github.com/apple/swift-evolution/blob/main/proposals/0347-type-inference-from-default-exprs.md)는 generic 매개변수 타입과 함께 기본 값을 사용하는 Swift의 기능을 확장한다. 이전에 Swift는 generic 타입이나 함수가 있는 경우 컴파일러가 오류를 발생시켰지만, 이제 기본 표현식에 대한 구체적인 타입을 제공할 수 있다. 예를 들어, 모든 종류의 `Sequence`에서 무작위 원소의 개수를 반환하는 함수가 있을 수 있다.

```swift
func drawLotto1<T: Sequence>(from options: T, count: Int = 7) -> [T.Element] {
  Array(options.shuffled().prefix(count))
}
```

이를 통해 문자열이나 정수 범위가 포함된 모든 종류의 `Sequence`를 사용하여 `drawLotto` 함수를 실행할 수 있다.

```swift
print(drawLotto1(from: 1...49))
print(drawLotto1(from: ["Jenny", "Trixie", "Cynthia"], count: 2))
```

SE-0347은 이를 확장하여 함수에서 `T` 매개변수의 기본값으로 구체적인 타입을 제공할 수 있다.

```swift
func drawLotto2<T: Sequence>(from options: T = 1...49, count: Int = 7) -> [T.Element] {
  Array(options.shuffled().prefix(count))
}
```

이제 사용자 지정 `Sequence`를 사용하여 함수를 호출하거나 기본값을 사용할 수 있다.

```swift
print(drawLotto2(from: ["Jenny", "Trixie", "Cynthia"], count: 2))
print(drawLotto2())
```