# Noncopyable structs and enums

Swift의 모든 타입은 복사할 수 있다. 즉, 하나의 인스턴스를 변수에 대입하거나 함수의 파라미터로 넘겨서 인스턴스를 복사할 수 있다. 클래스의 인스턴스는 초기화 할 때 고유 ID를 가지며, 고유한 인스턴스에 대한 참조만 복사돼서 고유한 리소스를 나타낼 수 있다. 따라서 모든 클래스의 인스턴스는 하나의 주소를 가리키고 있어서 공유 소유권이 요구된다. 만약 둘 이상의 스레드에서 동시에 자원에 접근한다면 결과 값에 영향을 줄 수 있는 Race condition이 발생할 수 있고, 이를 안전하게 사용하기 위해 추가적인 오버헤드도 발생한다. [SE-0390](https://github.com/apple/swift-evolution/blob/main/proposals/0390-noncopyable-structs-and-enums.md)에서는 복사할 수 없는 타입을 나타내는 `~Copyable` 프로토콜을 통해 고유한 소유권을 가진 고유한 리소스를 정의하는 매커니즘을 제안한다. 아래는 `~Copyable` 프로토콜을 채택한 `User` 구조체이다.

```swift
struct User: ~Copyable {
  var name: String
}
```

'복사할 수 없다'가 어떤 의미인지 이해하기 위해 아래의 코드를 살펴보자.

```swift
func createUser() {
  let newUser = User(name: "Anonymous")

  var userCopy = newUser
  print(userCopy.name)
}

createUser()
```

위 코드에서 `User` 타입의 인스턴스를 생성하고, 해당 인스턴스를 `userCopy` 변수에 대입했다. `User`는 noncopyable한 타입이므로, `userCopy` 변수에 대입할 때 복사하지 않고 소유권을 넘긴다. 따라서 `newUser`는 더 이상 사용할 수 없어서 컴파일 에러가 발생한다.

```swift
print(newUser.name) // 컴파일 에러
```

&nbsp;
## Borrowing

[SE-0377](https://github.com/apple/swift-evolution/blob/main/proposals/0377-parameter-ownership-modifiers.md)은 noncopyable 타입을 어떤 방식으로 사용할지 결정하는 modifier를 소개한다. 우선 `borrowing` 부터 살펴보자.

```swift
func createAndGreetUser() {
  let newUser = User(name: "Anonymous")
  greet(newUser)
  print("Goodbye, \(newUser.name)")
}

func greet(_ user: borrowing User) {
  print("Hello, \(user.name)!")
}

createAndGreetUser()
```

`borrowing`은 현재 소유자로부터 값의 소유권을 빼앗지 않는다. 따라서 `greet` 함수 내에서 `user` 파라미터의 값을 변경하는 것이 허용되지 않고 read-only 권한만 얻는다. 메서드의 파라미터 뿐만 아니라, 메서드 자체에도 `borrowing` modifier를 사용할 수 있다. `borrowing` 메서드는 `self` 매개변수의 소유권을 빌려서 사용한다.

```swift
let f: (borrowing Foo) -> Void = { a in a.foo() }
borrowing func foo() { }
...
```

&nbsp;
## Consuming

Consuming modifier는 noncopyable 타입의 소유권을 가져와서 사용한다.

```swift
func createAndGreetUser() {
  let newUser = User(name: "Anonymous")
  greet(newUser)
  print("Goodbye, \(newUser.name)") // 컴파일 에러
}

func greet(_ user: consuming User) {
  print("Hello, \(user.name)!")
}

createAndGreetUser()
```

위 코드에서 `print` 구문은 컴파일 에러가 발생한다. 왜냐하면 `greet` 함수에서 `newUser`에 대한 소유권을 가져가서 더 이상 사용할 수 없어졌기 때문이다. 그리고 `greet` 함수는 소유권을 가져왔으므로 `user`의 값을 변경하는 것이 허용된다. `consuming` modifier도 메서드에 붙일 수 있지만, `borrowing`과 다르게 nonescaping 클로저에는 붙일 수 없다. nonescaping 클로저는 항상 `borrowing`하게 동작하기 때문이다.

```swift
let f: (consuming Foo) -> Void = { a in a.foo() }
consuming func foo() { }

func foo(f: consuming () -> ()) { } // X
```

`consuming`은 소유권을 가져왔으니, 개체를 더 이상 사용하지 않을 때 `deinit` 할 책임을 갖는다. Noncopyable 타입은 클래스와 같이 `deinit`을 제공할 수 있지만, `discard` 혹은 `consume self`를 사용해서 명시적으로 `deinit`을 트리거할 수 있다. 해당 내용은 [Suppressing `deinit` in a `consuming` method](./suppressing-deinit-in-a-consuming-method.md)에 정리할 예정이다.

&nbsp;
## 참고 문헌

- [Hacking with Swift / What's new in Swift 5.9](https://www.hackingwithswift.com/articles/258/whats-new-in-swift-5-9)
- [Swift Evolution / SE-0390 Noncopyable structs and enums](https://github.com/apple/swift-evolution/blob/main/proposals/0390-noncopyable-structs-and-enums.md)