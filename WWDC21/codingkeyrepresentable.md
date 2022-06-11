## Allow coding of non String/Int keyed Dictionary into a KeyedContainer

[SE-0320](https://github.com/apple/swift-evolution/blob/main/proposals/0320-codingkeyrepresentable.md)은 `String` 또는 `Int`가 아닌 키가 있는 dictionary를 `UnkeyedContainer`가 아닌 `KeyedContainer`로 인코딩할 수 있도록 하는 새로운 `CodingKeyRepresentable`을 도입한다. 이것이 왜 중요한지 이해하려면 `CodingKeyRepresentable`가 없는 동작을 확인해야 한다. 예를 들어, 아래의 코드는 dictionary의 키에 대해 열거형 케이스를 사용하고, 이를 JSON으로 인코딩한 뒤 결과 문자열을 출력한다.

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

열거형에는 `String` raw 값이 있지만, dictionary의 키는 `String` 또는 `Int`가 아니므로 `["twitter", "@twostraws", "name", "Paul"]`이 출력된다. 이는 키/값 쌍이 아닌 4개의 개별적인 문자열들이다. Swift는 디코딩 시 이를 인식하고 각 키/값 쌍 내부에서 문자열을 `OldSettings`의 키 및 문자열 값과 매칭시키지만, JSON을 서버로 보내려는 경우에는 도움이 되지 않는다.

&nbsp;
## 솔루션

새로운 `CodingKeyRepresentable`은 이 문제를 해결하여 새로운 dictionary의 키가 올바르게 쓰여지도록 한다. 그러나 이렇게 하면 Codable JSON이 작성되는 방식이 변경되므로, 다음과 같이 `CodingKeyRepresentable`을 명시적으로 준수해야 한다.

```swift
enum NewSettings: String, Codable, CodingKeyRepresentable {
  case name
  case twitter
}

let newDict: [NewSettings: String] = [.name: "Paul", .twitter: "@twostraws"]
let newData = try! JSONEncoder().encode(newDict)
print(String(decoding: newData, as: UTF8.self))
```

이제 결과 문자열은 `{"twitter":"@twostraws","name":"Paul"}`가 된다. 이는 Swift 외부에서 훨씬 더 유용하다. 사용자 정의 구조체를 키로 사용하려는 경우, `CodingKeyRepresentable`을 준수하고 데이터를 문자열로 변환하는 고유한 메서드를 제공할 수도 있다.