## Implicitly opened existentials

[SE-0352](https://github.com/apple/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md)는 Swift가 프로토콜을 사용하여 일반 함수를 호출할 수 있는 범위를 늘려서 이전에 존재했던 다소 이상한 장벽을 제거한다. 예를 들어, 다음은 모든 종류의 `Numeric`으로 작동할 수 있는 *generic* 함수다:

```swift
func double<T: Numeric>(_ number: T) -> T {
  number * 2
}
```

만약 `double(5)`와 같이 직접적으로 호출하는 경우, Swift 컴파일러는 함수를 전문화 하도록 선택할 수 있다. 이는 성능상의 이유로 `Int`를 직접 받아들이는 버전을 효과적으로 생성한다. 그러나 SE-0352는 데이터가 다음과 같이 프로토콜을 준수한다는 사실만 알고 있을 때, 해당 함수를 호출할 수 있도록 허용한다.

```swift
let first = 1
let second = 2.0
let third: Float = 3

let numbers: [any Numeric] = [first, second, third]

for number in numbers {
  print(double(number))
}
```

- Swift는 다음과 같은 *existential types*를 호출한다: 실제로 사용 중인 데이터 타입은 상자 안에 있으며, 우리가 그 상자에서 메소드를 호출할 때 Swift는 상자 안의 데이터에 대한 메소드를 암시적으로 호출한다. 

- SE-0352는 이를 함수 호출에도 확장한다: for-loop의 `number` 값은 *existential type*(`Double`, `Float`, `Int`를 포함하는 상자)이지만, Swift는 상자 내부에 값을 전송하여 *generic* `double()` 함수에 전달할 수 있다.

하지만 아래와 같은 함수는 작동하지 않는다. 왜냐하면 Swift의 컴파일러가 두 값이 `==`을 사용하여 비교할 수 없다는 것을 컴파일 타입에 확인할 수 없기 때문이다.

```swift
func areEqual<T: Numeric>(_ a: T, _ b: T) -> Bool {
  a == b
}

print(areEqual(numbers[0], numbers[1]))
```