## Value and type parameter packs

Swift 5.9 이전에 메서를 작성할 때 같은 동작을 하더라도 파라미터의 갯수가 다르다면 오버로드 했다.

```swift
func eachFirst<T>(
  _ item: T
) -> T?

func eachFirst<T1, T2>(
  _ item1: T1,
  _ item2: T2
) -> (T1?, T2?)

func eachFirst<T1, T2, T3>(
  _ item1: T1,
  _ item2: T2,
  _ item3: T3
) -> (T1?, T2?, T3?)
```

이러한 반복을 줄이기 위해서 배열을 사용할 수 있지만, 배열의 타입이 다르다면 컴파일 에러가 발생한다.

```swift
func eachFirst<T: Collection>(collections: [T]) -> [T.Element?] {
  collections.map(\.first)
}

let numbers = [0, 1, 2]
let names = ["Antoine", "Maaike", "Sep"]
print(eachFirst(collections: [numbers, names])) // Confliciting arguments to generic parameter 'T' ('[String]' vs '[Int]')
```

위와 같이 제너릭 타입만 사용했을 때 발생하는 문제는 `Parameter packs`를 사용해서 해결할 수 있다.

```swift
func eachFirst<each T: Collection>(_ item: repeat each T) -> (repeat (each T).Element?) {
  return (repeat (each item).first)
}
```

&nbsp;
## 출처
- [SwiftLee](https://www.avanderlee.com/swift/value-and-type-parameter-packs/)