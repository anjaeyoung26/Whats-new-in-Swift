## Lightweight same-type requirements for primary associated types

[SE-0346](https://github.com/apple/swift-evolutions/blob/main/proposals/0346-light-weight-same-type-syntax.md)는 특정 `associatedtype`이 있는 프로토콜을 참조하기 위해 새로운 구문을 추가한다. 우선 특정 데이터를 캐시하는 프로토콜을 살펴보자:

```swift
protocol Cache<Content> {
  associatedtype Content

  var items: [Content] { get set }

  init(items: [Content])
  mutating func add(item: Content)
}
```

`Cache`는 `associatedtype`을 선언했지만 <>에 해당 타입을 명시해서 프로토콜과 generic 모두 해당되는 것처럼 보인다. 여기서 `Content`는 primary associated type라고 부른다. primary associated type이 어떻게 사용되는지 알아보자. 다음과 같이 `Cache` 프로토콜을 준수하는 구체적인 타입을 만든다:

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

이제 다음과 같이 특정 캐시를 직접 생성할 수 있다:

```swift
func loadDefaultCache() -> LocalFileCache {
  LocalFileCache(items: [])
}
```

하지만 구체적인 타입을 반환하지 않고 opaque type으로 세부 사항을 숨길 수 있다:

```swift
func loadDefaultCache() -> some Cache {
  LocalFileCache(items: [])
}
```

`some Cache`를 사용하면 반환할 캐시를 유연하게 변경할 수 있다. 그러나 primary associated type으로 할 수 있는 것은 구체적인 타입을 명시하거나 opaque type을 반환하는 것의 중간 지점을 제공하는 것이다. 따라서 다음과 같이 primary associated type을 사용해 반환 값을 변경할 수 있다:

```swift
func loadDefaultCacheNew() -> some Cache<File> {
  LocalFileCache(items: [])
}
```

여전히 `Cache` 프로토콜을 준수하는 다른 타입으로 변경할 수 있지만 내부적으로 파일을 저장한다는 점을 분명히 했다.

