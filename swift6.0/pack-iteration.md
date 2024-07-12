## Pack iteration

Swift 5.9에서 서로 다른 타입의 매개변수를 허용하는 함수를 작성할 수 있게 value and type parameter packs가 추가됐습니다. ([SE-0393](../swift5.9/value-and-type-parameter-packs.md)) 예를 들어, 아래와 같은 함수를 작성할 수 있습니다:

```swift
let numbers = [0, 1, 2]
let names = ["antoine", "Maaike", "Sep"]
let collections = [numbers, names]

func eachFirstGeneric<T: Collection>(collections: [T]) -> [T.Element?] {
  collections.map(\.first)
}
// 컴파일 에러
print(eachFirstGeneric(collections: [numbers, names]))

func eachFirst<each T: Collection>(_ item: repeat each T) -> (repeat (each T).Element?) {
  return (repeat (each item).first)
}
// Prints 0
print(eachFirst(collections: [numbers, names]))
```

이는 리스트 연산에서 short-circuiting을 허용하지 않아서 부자연스러운 부분이 있습니다. 우선 short-circuiting란, 결과가 정해지면 bool 표현식의 evaluate가 중지되는 동작을 의미합니다. 아래의 if 조건문에서 `isHungry` 값이 `false`이므로 다음 조건인 `isFoodAvailable`의 값을 확인하지 않는데, 이를 short-circuiting라고 합니다.

```swift
let isHungry = false
let isFoodAvailable = true
if isHungry && isFoodAvailable {
  eatFoods()
}
```

하지만 value parameter packs는 리스트 연산에서 모든 요소를 evaluate하기 때문에 short-circuiting을 허용하지 않습니다. 따라서 evaluate를 중지하는 유일한 방법은 패턴 표현식을 throw하는 함수/클로저로 표시하고 do/catch 구문에서 에러를 catch해서 반환하는 것인데, 이는 부자연스러운 코드로 보입니다.

```swift
struct NotEqual: Error {}

func == <each Element: Equatable>(lhs: (repeat each Element), rhs: (repeat each Element)) -> Bool {
  // Local throwing function for operating over each element of a pack expansion.
  func isEqual<T: Equatable>(_ left: T, _ right: T) throws {
    if left == right {
      return
    }
    
    throw NotEqual()
  }

  // Do-catch statement for short-circuiting as soon as two tuple elements are not equal.
  do {
    repeat try isEqual(each lhs, each rhs)
  } catch {
    return false
  }

  return true
}
```

이를 해결하기 위해 [SE-0408](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0408-pack-iteration.md)은 for-in 구문에서 parameter packs를 반복하도록 허용합니다.

```swift
func == <each Element: Equatable>(lhs: (repeat each Element), rhs: (repeat each Element)) -> Bool {
  for (left, right) in repeat (each lhs, each rhs) {
    guard left == right else { return false }
  }
  return true
}
```