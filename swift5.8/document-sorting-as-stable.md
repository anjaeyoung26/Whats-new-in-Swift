## Document sorting as stable

Swift의 정렬 알고리즘은 Swift 5 버전 이전에 안정적으로 변경되었다. 안정적인 정렬은 동일하거나 정렬되지 않은 것으로 비교되는 모든 요소에 대해 원래 상대 순서를 유지하는 정렬이다. 예를 들어, 이미 성(`last`)를 기준으로 정렬된 플레이어 목록에서 이름(`first`)을 기준으로 정렬하면 "Ashley"라는 두 플레이어의 원래 순서가 유지된다.

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
//    Player(first: "Ashley", last: "Hatch"),
//    Player(first: "Ashley", last: "Sanchez"),
//    Player(first: "Kristie", last: "Mewis"),
//    Player(first: "Sam", last: "Coffey"),
//    Player(first: "Sophia", last: "Smith"),
// ]
```

하지만 Swift의 `sort` 함수는 "The sorting algorithm is not guaranteed to be stable. A stable sort preserves the relative order of elements that compare as equal."라는 주석이 달려있다. 이미 안정적으로 정렬되고 있지만 문서 상에서는 안정적이지 않다고 명시되어 있다. [SE-0372](https://github.com/apple/swift-evolution/blob/main/proposals/0372-document-sorting-as-stable.md)는 주석을 다음과 같이 변경하자고 제안한다.

```
- /// The sorting algorithm is not guaranteed to be stable. A stable sort
+ /// The sorting algorithm is guaranteed to be stable. A stable sort
  /// preserves the relative order of elements that compare as equal.
```