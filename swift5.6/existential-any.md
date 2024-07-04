## Existential any

[SE-0335](https://github.com/apple/swift-evolution/blob/main/proposals/0335-existential-any.md)는 existential type을 위한 새로운 `any` 키워드를 도입했다.

```swift
protocol P {}
protocol Q {}
struct S: P, Q {}

let p1: P = S()
```

위 코드에서 `P`는 existential type으로 프로토콜을 준수하는 모든 타입을 `p1`에 할당할 수 있다.

```swift
protocol P {}
struct S: P {}
class C: P {}

var p1: P = S()
p1 = C()
```

위 코드를 실행하면 Swift는 `p1`의 타입을 런타임에 결정한다. 왜냐하면 `p1`은 다른 `P` 프로토콜을 준수하는 타입으로 언제든 변경될 수 있기 때문이다. 이는 concrete type을 사용하는 정적 디스패치에 비해 더 많은 비용이 요구된다. 하지만 existential type을 명확하게 나타내는 수단이 없다.

&nbsp;
## 솔루션

Swift 5.6은 existential type을 나타내는 `any` 키워드가 추가된다. 이는 existential type을 명시적으로 나타내는 용도이지만, 이후 버전에서는 `any`를 명시하지 않으면 경고가 표시되고 Swift 6 부터는 에러가 표시된다.

```swift
protocol Vehicle {}

func travel(using vehicle: any Vehicle) {}
```
