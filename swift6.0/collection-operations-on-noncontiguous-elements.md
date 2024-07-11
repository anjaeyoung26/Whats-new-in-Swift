## Add Collection Operations on Noncontiguous Elements

Swift 6.0 이전에는 collection 내의 그룹을 참조할 때 `Range<Index>` 타입을 사용했습니다. 하지만 collection 내의 불연속적인 위치를 참조하는 API를 제공하지 않습니다. 예를 들어, Swift 6.0 이전에서 collection 내에 짝수들의 index를 구하는 코드입니다:

```swift
var numbers = Array(1...15)

var indicesOfEvens: [Int] = []
for (index, number) in numbers.enumerated() {
  if number.isMultiple(of: 2) {
    indicesOfEvens.append(index)
  }
}
print(indicesOfEvens) // [1, 3, 5, 7, 9, 11, 13]
```

또한 짝수, 홀수 순서로 collection을 재배치하는 코드입니다.

```swift
var evenNumbers: [Int] = []
var otherNumbers: [Int] = []
for number in numbers {
  if number.isMultiple(of: 2) {
    evenNumbers.append(number)
  } else {
    otherNumbers.append(number)
  }
}
numbers = evenNumbers + otherNumbers
print(numbers) // [2, 4, 6, 8, 10, 12, 14, 1, 3, 5, 7, 9, 11, 13, 15]
```

연산이 복잡해질수록 처리하는 과정이 번거로워집니다. 이를 해결하기 위해 [SE-0270](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0270-rangeset-and-collection-operations.md)은 `RangeSet` 타입을 추가해서 collection 내의 연속되지 않은 위치를 참조하는 API를 제안합니다.

```swift
var numbers = Array(1...15)

// 모든 짝수의 index
let indicesOfEvens = numbers.indices(where: { $0.isMultiple(of: 2) })

// 모든 짝수의 합
let sumOfEvens = numbers[indicesOfEvens].reduce(0, +) // 54

// 짝수, 홀수 순서로 재배치
let rangeOfEvens = numbers.moveSubranges(indicesOfEvens, to: numbers.startIndex)
// numbers == [2, 4, 6, 8, 10, 12, 14, 1, 3, 5, 7, 9, 11, 13, 15]
// numbers[rangeOfEvens] == [2, 4, 6, 8, 10, 12, 14]
```