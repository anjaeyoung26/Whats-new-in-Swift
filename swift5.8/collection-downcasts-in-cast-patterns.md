## Collection downcasts in cast patterns

Swift 5.8 부터는 캐스트 패턴의 컬렉션 다운 캐스팅이 지원된다. 예를 들어, 아래의 코드는 Swift 5.8에서 유효하지만 이전 버전에서는 동작하지 않는다.

```swift
class Pet { }
class Dog: Pet {
  func bark() { print("Woof!") }
}
    
func bark(using pets: [Pet]) {
  switch pets {
  case let pets as [Dog]:
    for pet in pets {
      pet.bark()
    }
  default:
    print("No barking today.")
  }
}
```

Swift 5.8 이전 버전에서는 “Collection downcast in cast pattern is not implemented; use an explicit downcast to '[Dog]' instead.” 에러가 표시된다. switch 문에서 `[Pet]` 타입의 배열을 자식 클래스인 `[Dog]` 타입으로 다운 캐스팅하는 부분에서 에러가 발생한다. 바로 컬렉션에서 다운 캐스팅이 지원되지 않았기 때문인데, Swift 5.8 버전 부터는 지원되서 유효하게 작동한다.