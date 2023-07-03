## Add `sleep(for:)` to Clock

기존의 `Clock`은 미래의 instant 까지 sleep하는 방법을 제공했다.

```swift
func sleep(until deadline: Instant, tolerance: Instant.Duration?) async throws
```

하지만 일정 duration 동안 sleep하는 기능은 제공하지 않았다. 이는 `Clock`이 포함된 앱의 기능을 테스트하는 데 불편함을 야기했다.

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
```

위 클래스를 테스트하기 위해서는 실제로 5초를 기다려서 결과를 확인하는 방법밖에 없었다.

```swift
let model = FeatureModel()

XCTAssertEqual(model.message, nil)
await model.onAppear()

// Waits for 5 seconds

XCTAssertEqual(model.message, "Welcome!")
```

이는 테스트를 작성하지 않는 사람들에게도 영향을 미친다. 해당 기능을 Xcode preview에 넣으면 welcome 메시지를 보기 위해 5초 동안 기다려야 한다. 이러한 문제를 해결하기 위해 제어하기 어려운 `Task.sleep` 대신에 `Clock`을 주입해보자.

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

`Clock`을 주입하는 방법도 해결책이 될 수 없다. 왜냐하면 `Clock`은 Existential 타입으로 주입받기 때문에 `self.clock.now` 프로퍼티에 접근할 수 없다. 대신 `Clock`에 `sleep(for:)` 메서드를 추가해서 위와 같은 문제를 해결할 수 있다.

```swift
class FeatureModel: ObservableObject {
  @Published var message: String?
  let clock: any Clock<Duration>

  func onAppear() async {
    do {
      try await self.clock.sleep(for: .seconds(5))
      self.message = "Welcome!"
    } catch {}
  }
}
```

&nbsp;
## 참고 문헌
- [Swift Evolution / SE-0374 Add sleep(for:) to Clock](https://github.com/apple/swift-evolution/blob/main/proposals/0374-clock-sleep-for.md)
- [Hacking with Swift / What's new in Swift 5.9](https://www.hackingwithswift.com/articles/258/whats-new-in-swift-5-9)