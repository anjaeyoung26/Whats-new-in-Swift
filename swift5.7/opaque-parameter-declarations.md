## Opaque parameter declarations

[SE-0341](https://github.com/apple/swift-evolution/blob/main/proposals/0341-opaque-parameters.md)는 `some` 키워드와 함께 generic 매개변수를 선언할 수 있게 한다:

```swift
func isSorted(array: [some Comparable]) -> Bool {
  array == array.sorted()
}
```

이는 Swift 5.6까지 다음과 같이 작성해왔다:

```swift
func isSortedOld<T: Comparable>(array: [T]) -> Bool {
  array = array.sorted()
}
```

some generic 구문이 비교적 단순하지만 제약 조건을 추가할 수 없다는 단점이 있다. 예를 들어, `isSortedOld`는 다음과 같이 제약 조건을 추가할 수 있다:

```swift
func isSortedOld<T: Comparable & Numeric>(array: [T]) -> Bool {
  array = array.sorted()
}
```

하지만 some generic 구문은 하나의 제약 조건만 명시할 수 있다.
