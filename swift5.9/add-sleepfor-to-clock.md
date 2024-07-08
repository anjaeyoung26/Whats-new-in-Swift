## Add sleep(for:) to Clock

기존의 `Clock`은 미래의 instant까지 sleep 하는 방법을 제공했다.

```swift
func sleep(until deadline: Instant, tolerance: Instant.Duration?) async throws
```

하지만 일정 duration 동안 sleep 하는 기능을 제공하지 않아서 불편함을 야기한다. 우선 `Clock` 이전의 `Task`를 사용하는 코드를 살펴보자:

```swift
class FeatureModel: ObservableObject {
  @Published var message: String?
  func onAppear() async {
    do {
      try await Task.sleep(until: .now.advanced(by: .seconds(5)))
      self.message = "Welcome!"
    } catch {}
  }
}

let model = FeatureModel()

XCTAssertEqual(model.message, nil)
await model.onAppear()

// Waits for 5 seconds
XCTAssertEqual(model.message, "Welcome!")
```

`FeatureModel`의 동작을 테스트하려면 실제로 5초를 기다려서 결과를 확인해야 한다. 이러한 문제는 `Task.sleep`을 `Clock`로 대체해도 해결할 수 없었다. 왜냐하면 `Clock`은 existential type을 주입받아서 `self.clock.now` 프로퍼티에 접근할 수 없기 때문이다.

```swift
class FeatureModel: ObservableObject {
  @Published var message: String?
  let clock: any Clock<Duration>

  func onAppear() async {
    do {
      try await self.clock.sleep(until: self.clock.now.advanced(by: .seconds(5))) // X
      self.message = "Welcome!"
    } catch {}
  }
}
```

이는 Swift 5.9에서 추가된 `Clock`의 `sleep(for:)` 메서드로 문제를 해결할 수 있다.

```swift
extension Clock {

  public func sleep(
    for duration: Duration,
    tolerance: Duration? = nil
  ) async throws {
    try await self.sleep(until: self.now.advanced(by: duration), tolerance: tolerance)
  }
}

class FeatureModel: ObservableObject {
  @Published var message: String?
  let clock: any Clock<Duration>

  func onAppear() async {
    do {
      try await self.clock.sleep(for: .seconds(5)) // O
      self.message = "Welcome!"
    } catch {}
  }
}
```

&nbsp;
## 참고 문헌
- [Swift Evolution / SE-0374 Add sleep(for:) to Clock](https://github.com/apple/swift-evolution/blob/main/proposals/0374-clock-sleep-for.md)
- [Hacking with Swift / What's new in Swift 5.9](https://www.hackingwithswift.com/articles/258/whats-new-in-swift-5-9)
