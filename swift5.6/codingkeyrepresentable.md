## Allow coding of non String/Int keyed Dictionary into a KeyedContainer

[SE-0320](https://github.com/apple/swift-evolution/blob/main/proposals/0320-codingkeyrepresentable.md)은 `String` 또는 `Int` 외에 다른 타입의 키를 가진 dictionary를 `UnkeyedContainer`가 아닌 `KeyedContainer`로 인코딩할 수 있게 하는 `CodingKeyRepresentable`을 도입한다. 이것이 왜 중요한지 이해하기 위해 `CodingKeyRepresentable`이 없는 경우의 동작을 확인해야 한다. 예를 들어, 아래의 코드는 dictionary의 키로 enum을 사용하고 JSON으로 인코딩한다.

```swift
import Foundation

enum OldSettings: String, Codable {
  case name
  case twitter
}

let oldDict: [OldSettings: String] = [.name: "Paul", .twitter: "@twostraws"]
let oldData = try! JSONEncoder().encode(oldDict)
print(String(decoding: oldData, as: UTF8.self))
```

열거형에는 `String` raw 값이 있지만 "["twitter", "@twostraws", "name", "Paul"]"이 출력된다. 이는 키/값 쌍이 아닌 4개의 개별적인 문자열이다. Swift는 디코딩 시 `OldSettings`의 키와 문자열 값과 매칭시키지만 JSON을 서버로 보내려는 경우에는 유용하지 않다.

&nbsp;
## 솔루션

`CodingKeyRepresentable`은 이 문제를 해결하여 dictionary의 키가 올바르게 쓰여지도록 한다.

```swift
enum NewSettings: String, Codable, CodingKeyRepresentable {
  case name
  case twitter
}

let newDict: [NewSettings: String] = [.name: "Paul", .twitter: "@twostraws"]
let newData = try! JSONEncoder().encode(newDict)
print(String(decoding: newData, as: UTF8.self))
```

이제 "{"twitter":"@twostraws","name":"Paul"}"와 같이 출력된다. 이는 Swift 외부에서 훨씬 더 유용하다. 사용자 정의 구조체를 키로 사용하려는 경우 `CodingKeyRepresentable`을 준수하고 데이터를 문자열로 변환하는 고유한 메서드를 제공할 수도 있다.
