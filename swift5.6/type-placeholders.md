## Type placeholders

[SE-0315](https://github.com/apple/swift-evolution/blob/main/proposals/0315-placeholder-types.md)는 *type placeholders*의 개념을 도입하여 값 타입의 일부만 명시적으로 지정할 수 있으므로, 나머지는 타입 유추를 사용하여 채울 수 있다. 실제로 이것은 Swift가 타입 추론을 사용하려는 위치에 `_`를 입력하는 것을 의미한다. 즉, 다음 세 줄의 코드가 동일하다:

```swift
let score1 = 5
let score2: Int = 5
let score3: _ = 5
```

위와 같이 간단한 예에서 *type placeholders*는 아무것도 추가하지 않지만, 컴파일러가 타입의 일부를 올바르게 추론할 수 있을 때 유용하다. 하지만 모든 경우가 그렇지는 않다. 예를 들어, 학생 이름과 시험 점수를 포함하는 dictionary를 만드는 경우 다음과 같이 작성할 수 있다:

```swift
var results1 = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```

Swift는 문자열을 key로 사용하고 `Any`의 배열을 value로 사용하는 dictionary라고 추론할 것이다. 다음과 같이 전체 타입을 명시적으로 지정할 수 있다:

```swift
var results2: [String: [Int]] = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```

그러나 *type placeholders*를 사용하면 컴파일러가 추론할 부분 대신 `_`를 쓸 수 있다. 이는 *'우리가 선택한 정확한 타입으로 추론해야 한다'*라고 명시적으로 말하는 방법이다.

```swift
var results3: [_ : [Int]] = [
  "Cynthia": [],
  "Jenny": [],
  "Trixie": [],
]
```

보다시피 `_` 타입 추론에 대한 명시적 요청이 있지만, 우리는 여전히 정확한 배열 타입을 지정할 기회가 있다. 만약 Swift가 타입을 `Optional`로 유추하려면 `_?`를 사용할 수 있다. 

&nbsp;

## In function signature
*Type placeholders*는 우리가 함수 서명을 작성하는 방식에 영향을 미치지 않는다. 여전히 해당 매개변수와 반환 타입을 완전히 제공해야 한다. 예를 들어, 다음과 같이 `Player`를 생성하는 코드를 작성할 수 있다:

```swift
struct Player<T: Numeric> {
  var name: String
  var score: T
}

func createPlayer() -> _ {
  player(name: "Anonymous", score: 0)
}
```

컴파일러는 `createPlayer`에 대한 오류를 발생할 것이다. 이는 반환 타입을 명시적으로 지정하지 않고, *type placeholders*를 통해 컴파일러에게 반환 타입을 추론하라고 요청했기 때문이다. 하지만 Xcode의 *Fix-it*은 `createPlayer`의 반환 타입인 `_`를 `Player<Int>`로 대체하라고 제안할 것이다. 이는 더 복잡한 타입을 처리할 때 상당한 양의 번거로움을 줄일 수 있다.

*Type placeholders*는 덜 관련성이 있거나 boilerplate 부분을 `_`로 모두 교체하고 중요한 부분을 철자로 남겨두어 코드를 더 읽기 쉽게 만들 수 있다. 이는 *type placeholders*를 통해 타입과 관련된 긴 주석을 단순화할 수 있는 방법이다.