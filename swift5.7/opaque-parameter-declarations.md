## Opaque parameter declarations

[SE-0341](https://github.com/apple/swift-evolution/blob/main/proposals/0341-opaque-parameters.md)는 *generic*이 사용되는 곳에서 `some`과 함께 매개변수를 선언할 수 있는 기능을 소개한다. 예를 들어, 배열이 정렬되었는지 확인하는 함수를 작성하려는 경우 Swift 5.7 이상에서는 다음과 같이 작성할 수 있다:

```swift
func isSorted(array: [some Comparable]) -> Bool {
  array == array.sorted()
}
```

`[some Comparable]` 매개변수 타입의 의미는 *'이 함수는 `Comparable` 프로토콜을 준수하는 한 타입의 요소를 포함하는 배열과 함께 작동한다'*이다. 이는 Swift 5.6까지 *generic* 타입의 매개변수를 받는 함수로 작성해왔다:

```swift
func isSortedOld<T: Comparable>(array: [T]) -> Bool {
  array = array.sorted()
}
```

이 단순화된 *generic* 구문은 우리의 타입에 더 복잡한 제약 조건을 추가할 수 없음을 의미한다. 왜냐하면 합성된 *generic* 매개변수에 대한 특정 이름이 없기 때문이다. 예를 들어, `isSortedOld` 함수에서 `T`에 대해 `Comparable` 외에 제약 조건을 추가할 수 있지만, `isSorted`는 하나의 제약 조건만을 설정할 수 있다.
