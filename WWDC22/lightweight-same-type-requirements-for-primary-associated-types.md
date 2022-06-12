## Lightweight same-type requirements for primary associated types

[SE-0346](https://github.com/apple/swift-evolutions/blob/main/proposals/0346-light-weight-same-type-syntax.md)는 특정 `associatedtype`이 있는 프로토콜을 참조하기 위해 더 새롭고 단순한 구문을 추가한다. 예를 들어, 데이터의 종류에 따라 다른 방법으로 캐시하는 코드를 작성하는 경우 다음과 같이 시작할 수 있다:

```swift
protocol Cache<Content> {
  associatedtype Content

  var items: [Content] { get set }

  init(items: [Content])
  mutating func add(item: Content)
}
```

`Cache` 는 프로토콜과 generic 타입 둘 다 해당되는 것 처럼 보인다. 일치하는 타입이 채워야 하는 일종의 구멍을 `associatedtype`으로 선언했지만, <> 괄호 안에 해당 타입을 나열한다. <> 안의 부분은 Swift의 *primary associated type* 이라고 부르며, 이는 모든 `associatedtype`이 <> 안에 선언되어야 하는 게 아니라는 점이 중요하다. 대신에, dictionary의 키/값의 타입 또는 `Identifiable`의 식별자 타입과 같이 호출 코드와 연관되어 있는 항목을 나열해야 한다.

&nbsp;

이 시점에서 이전과 같이 프로토콜을 사용할 수 있다. 캐시하려는 데이터를 생성한 뒤, 다음과 같이 프로토콜을 준수하는 구체적인 캐시 타입을 생성할 수 있다.

```swift
struct File {
  let name: String
}

struct LocalFileCache: Cache {
  var items: [File]()

  mutating func add(item: File) {
    items.append(item)
  }
}
```

이제 캐시를 생성할 때, 다음과 같이 특정 캐시를 직접 생성할 수 있다:

```swift
func loadDefaultCache() -> LocalFileCache {
  LocalFileCache(items: [])
}
```

하지만 구체적인 캐시 타입을 반환하는 것이 아닌, 우리가 하는 일에 세부 사항을 숨기고 싶을 때가 종종 있다:

```swift
func loadDefaultCache() -> some Cache {
  LocalFileCache(items: [])
}
```

`some Cache`를 사용하면 어떤 종류의 캐시가 반환되는지에 대한 생각을 유연하게 변경할 수 있다. 그러나 우리가 SE-0346으로 할 수 있는 것은 구체적인 타입을 특정하거나 불투명한(opaque) 타입을 반환하는 것 사이의 중간 지점을 제공하는 것이다. 따라서 다음과 같이 프로토콜을 전문화할 수 있다:

```swift
func loadDefaultCacheNew() -> some Cache<File> {
  LocalFileCache(items: [])
}
```

여전히 `Cache` 프로토콜을 준수하는 다른 타입으로 이동할 수 있지만, 여기에서 선택한 항목이 내부적으로 파일을 저장한다는 점을 분명히 했다.

