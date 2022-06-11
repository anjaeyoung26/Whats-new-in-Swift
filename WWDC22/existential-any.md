## Existential 타입이란?

[SE-0335](https://github.com/apple/swift-evolution/blob/main/proposals/0335-existential-any.md)는 existential 타입을 표시하기 위해 새로운 `any` 키워드를 도입했다. 기존의 프로토콜은 준수하는 타입이 구현해야 하는 메서드와 같이 일련의 요구 사항을 지정할 수 있다. 그래서 우리는 종종 다음과 같은 코드를 작성한다:

```swift
protocol Vehicle {
  func travel(to destination: String)
}

struct Car: Vehicle {
  func travel(to destination: String) {
    print("I'm driving to \(destination)")
  }
}

let vehicle = Car()
vehicle.travel(to: "London")
```

또한 프로토콜을 함수의 generic 타입 제약 조건으로 사용할 수 있습니다. 즉, 특정 프로토콜을 준수하는 모든 종류의 데이터와 함께 작동할 수 있는 코드를 작성합니다. 예를 들어, 이것은 `Vehicle`을 준수하는 모든 종류의 타입에 작동한다.

```swift
func travel<T: Vehicle>(to destinations: [String], using vehicle: T) {
  for destination in destinations {
    vehicle.travel(to: destination)
  }
}

travel(to: ["London", "Amarillo"], using: vehicle)
```

해당 코드가 컴파일되면 Swift는 `Car` 인스턴스로 `travel()`을 호출하는 것을 볼 수 있으므로, `travel()` 함수를 직접 호출하는 최적화된 코드를 생성할 수 있습니다. 이것은 정적 디스패치로 알려져 있습니다. 프로토콜을 사용하는 두 번째 방법이 있고 지금까지 사용한 다른 코드와 매우 유사해 보이기 때문에, 이 모든 것이 중요하다.

```swift
let vehicle2: Vehicle = Car()
vehicle2.travel(to: "Glasgow")
```

여기서 우리는 여전히 `Car` 구조체를 생성하고 있지만, `Vehicle`으로 저장하고 있다. 이는 기본 정보를 숨기는 단순한 문제가 아니라, 이 `Vehicle` 타입은 Existential 타입이라고 하는 완전히 다른 것이다. 즉, `Vehicle` 프로토콜을 준수하는 모든 유형의 값을 보유할 수 있는 새로운 데이터 유형이다. Existential 타입의 중요한 점은 `some` 키워드를 사용하는 불투명한 타입과 다르다. 항상 지정한 제약 조건을 준수하는 하나의 특정 타입을 나타내야 한다.

&nbsp;
## Existential 타입의 문제점
Existential 타입은 다음과 같이 함수와 함께 사용할 수도 있다:

```swift
func travle2(to destinations: [String], using vehicle: Vehicle) {
  for destination in destinations {
    vehicle.travel(to: destination)
  }
}
```

다른 `travel()` 함수와 비슷해 보일 수 있지만, 이 함수는 모든 종류의 `Vehicle` 객체를 허용하므로 Swift는 더 이상 동일한 최적화 세트를 수행할 수 없다. 동적 디스패치라는 프로세스를 사용해야 하며, 이는 동등한 generic에서 사용할 수 있는 정적 디스패치보다 효율성이 떨어진다. 따라서 Swift는 두 가지 프로토콜의 사용이 매우 유사해 보이는 위치에 있었고, 실제로는 더 느리고 existential 버전의 함수가 작성하기 더 쉬웠다.

&nbsp;
## 솔루션
이 문제를 해결하기 위해 Swift 5.6은 existential 타입과 함께 사용할 수 있는 새로운 `any` 키워드를 도입하여 코드에서 existential의 영향을 명시적으로 인정하고 있다. Swift 5.6에서는 이 새로운 동작이 활성화되고 작동하지만 향후 Swift 버전에서는 이를 사용하지 않으면 경고가 표시되며, Swift 6부터는 오류를 발생시킨다고 한다.(`any`를 사용하여 existential 타입을 표시해야 한다.)

따라서 다음과 같이 작성한다:

```swift
let vehicle3: any Vehicle = Car()
vehicle3.travel(to: "Glasgow")

func travel3(to destinations: [String], using vehicle: any Vehicle) {
  for destination in destinations {
    vehicle.travel(to: destination)
  }
}
```