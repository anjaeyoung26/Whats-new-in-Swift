## Document sorting as stable

Swift의 정렬 알고리즘은 Swift 5이전에 안정적으로 변경되었다. 예를 들어, 아래의 두 Ashely는 이미 `last`를 기준으로 정렬되었다. 이후 `first`를 기준으로 다시 정렬할 때 두 플레이어의 원래 순서가 유지된다.

```swift
var roster = [
   Player(first: "Sam", last: "Coffey"),
   Player(first: "Ashley", last: "Hatch"),
   Player(first: "Kristie", last: "Mewis"),
   Player(first: "Ashley", last: "Sanchez"),
   Player(first: "Sophia", last: "Smith"),
]

roster.sort(by: { $0.first < $1.first })
// roster == [
//    last 기준으로 정렬된 Hatch, Sanchez 순서가 유지된다.
//    Player(first: "Ashley", last: "Hatch"),
//    Player(first: "Ashley", last: "Sanchez"),
//    Player(first: "Kristie", last: "Mewis"),
//    Player(first: "Sam", last: "Coffey"),
//    Player(first: "Sophia", last: "Smith"),
// ]
```

하지만 Swift의 `sort` 함수는 "The sorting algorithm is not guaranteed to be stable. A stable sort preserves the relative order of elements that compare as equal."라는 주석이 달려있다. 이미 안정적으로 정렬되고 있지만 문서상에서는 안정적이지 않다고 명시되어 있다. [SE-0372](https://github.com/apple/swift-evolution/blob/main/proposals/0372-document-sorting-as-stable.md)는 주석을 다음과 같이 변경하자고 제안한다.

```
- /// The sorting algorithm is not guaranteed to be stable. A stable sort
+ /// The sorting algorithm is guaranteed to be stable. A stable sort
  /// preserves the relative order of elements that compare as equal.
```
