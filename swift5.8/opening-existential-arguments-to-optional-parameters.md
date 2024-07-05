## Opening existential arguments to optional parameters

[SE-0375](https://github.com/apple/swift-evolution/blob/main/proposals/0375-opening-existential-optional.md)는 프로토콜로 generic 함수를 호출할 수 있게 하는 기능을 확장한다. 해당 기능은 Swift 5.7의 [SE-0352](https://github.com/apple/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md)의 [Implicitly opened existentials](https://github.com/anjaeyoung26/WWDC/blob/main/WWDC22/implicitly-opened-existentials.md)으로 아래와 같은 코드를 작성할 수 있게 한다:

```swift
func double<T: Numeric>(_ number: T) -> T {
  number * 2
}

let first = 1
let second = 2.0
let third: Float = 3

let numbers: [any Numeric] = [first, second, third]

for number in numbers {
  print(double(number))
}
```

Swift 5.7에서 매개변수가 Optional이면 동작하지 않는 문제가 있었지만, Swift 5.8은 기능을 확장시켜 동작할 수 있도록 한다.

```swift
func optionalDouble<T: Numeric>(_ number: T?) -> T {
  let numberToDouble = number ?? 0
  return  numberToDouble * 2
}
    
for number in numbers {
  print(optionalDouble(number)) // “Type 'any Numeric' cannot conform to 'Numeric’”
}
```
