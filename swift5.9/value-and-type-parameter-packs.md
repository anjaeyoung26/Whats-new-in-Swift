## Value and type parameter packs

Swift 5.9 이전에는 동일한 동작을 하더라도 매개변수의 개수가 다르다면 메서드를 overload 했다:

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

이는 코드가 중복되는 문제점이 있다. 이러한 반복을 줄이기 위해 배열을 사용할 수 있지만, 만약 타입이 다르다면 컴파일 에러가 발생한다:

```swift
func eachFirst<T: Collection>(collections: [T]) -> [T.Element?] {
  collections.map(\.first)
}

let numbers = [0, 1, 2]
let names = ["Antoine", "Maaike", "Sep"]
print(eachFirst(collections: [numbers, names])) // Confliciting arguments to generic parameter 'T' ('[String]' vs '[Int]')
```

이는 Swift 5.9의 Parameter packs로 해결할 수 있다. Parameter packs는 generic과 비슷해 보이지만 여러 타입의 item을 사용할 수 있게 한다.

```swift
func eachFirst<each T: Collection>(_ item: repeat each T) -> (repeat (each T).Element?) {
  return (repeat (each item).first)
}

let numbers = [0, 1, 2]
let names = ["Antoine", "Maaike", "Sep"]
print(eachFirst(collections: [numbers, names])) // O
```

&nbsp;
## 출처
- [SwiftLee Value and Type parameter packs in Swift explained with examples](https://www.avanderlee.com/swift/value-and-type-parameter-packs/)
