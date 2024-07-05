## Collection downcasts in cast patterns

Swift 5.8부터는 컬렉션 다운 캐스팅이 지원된다. `bark` 함수의 매개변수인 `pets`는 `[Pet]`의 배열인데 하위 클래스인 `[Dog]`로 다운 캐스팅할 수 있다.

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

Swift 5.7에서는 위 코드를 실행하면 "Collection downcast in cast pattern is not implemented; use an explicit downcast to '[Dog]' instead." 컴파일 에러가 발생한다.
