## Unlock existentials for all protocols

[SE-0309](https://github.com/apple/swift-evolution/blob/main/proposals/0309-unlock-existential-types-for-all-protcols.md)는 `Self` 또는 `associatedtype`의 요구 사항이 있는 프로토콜을 타입으로 사용하는 것에 대한 Swift의 금지를 완화하여, 수행하는 작업에 따라 특정 프로퍼티 또는 메소드만 제한되는 모델로 이동한다. 간단히 말해서 아래와 같은 코드를 작성할 수 있다:

```swift
let firstName: any Equatable = "Paul"
let lastName: any Equatable = "Hudson"
```

`Equatable`은 `Self` 요구 사항이 있는 프로토콜로, 이를 채택하는 특정 타입을 참조하는 기능을 제공한다. 예를 들어, `Int` 타입은 `Equatable` 프로토콜을 준수하므로 `4 == 4`라고 말할 때, 실제로 두 개의 정수를 확인하여 값이 일치하는 경우 `true`를 반환하는 함수를 실행하는 것이다. Swift는 `func ==(first: Int, second: Int) -> Bool`과 유사한 함수를 사용하여 이 기능을 구현할 수 있지만, 확장성이 좋지 않다. 왜냐하면 비교하는 대상이 `Int`일 뿐만 아니라 Booleans, 문자열, 배열 등이 될 수 있으므로, 이를 모두 처리하기 위해서는 수십 개의 함수를 작성해야 하기 때문이다. 따라서 `Equatable` 프로토콜에는 `func ==(lhs: Self, rhs: Self) -> Bool`과 같은 요구 사항이 있다.

&nbsp;

이와 유사한 문제를 피하기 위해 Swift 5.7 이전의 프로토콜에서 `Self`가 나타날 때, 컴파일러는 다음과 같은 코드에서 `Self`를 허용하지 않았다:

```swift
let tvShow: [any Equatable] = ["Brooklyn", 99]
```

Swift 5.7부터 이 코드가 허용된다. 이때 `==`가 작동하려면 동일한 타입의 두 인스턴스가 있어야 하고, `any Equatable`을 사용하여 데이터의 정확한 유형을 숨기고 있기 때문에 `firstName == lastName`을 작성할 수 없다. 그러나 우리는 데이터에 대한 런타임 검사를 수행하여 작업 중인 항목을 구체적으로 식별하는 기능을 얻는다. `tvShow`와 같이 여러가지 타입이 섞여 있는 혼합 배열의 경우 다음과 같이 작성할 수 있다:

```swift
for item in tvShow {
  if let item = item as? String {
    print("Found string: \(item)")
  } else if let item = item as? Int {
    print("Found integer: \(item)")
  }
}
```

또는 `firstName`과 `lastName`의 경우 다음과 같이 사용할 수 있다:

```swift
if let firstName = firstName as? String, let lastName = lastName as? String {
  print(firstName == lastName)
}
```

또한 `Sequence`의 모든 항목이 `Identifiable` 프로토콜을 준수하는지 확인하는 코드를 작성할 수 있다:

```swift
func canBeIdentified(_ input: any Sequence) -> Bool {
  input.allSatisfy { $0 is any Identifiable }
}
```


