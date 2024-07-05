## Clock, Instant, and Duration

[SE-0329](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md)는 Swift에서 시간과 기간(duration)을 참조하는 새로운 방식을 제안한다.(iOS 16+)

### Clock

`Clock`은 흘러간 시간을 측정하고 지정된 시점까지 작업을 지연시키는 매커니즘이다.

```swift
public protocol Clock: Sendable {
  associatedtype Duration: DurationProtocol
  associatedtype Instant: InstantProtocol where Instant.Duration == Duration

  // 현재 시간을 나타내며, `.now + .seconds(1)`과 같이 기준점으로 사용된다.
  var now: Instant { get }

  // 최소 시간 해상도를 나타내며, 기본 값은 `.nanosecond(1)`이다. 이는 1 나노초 단위로 시간 경과를 측정한다는 의미이다.
  var minResolution: Instant.Duration { get }

  static var continuous: ContinuousClock
  static var suspending: SuspendingClock

  func sleep(until deadline: Instant, tolerance: Instant.Duration?) async throws
}

// 지정된 시점까지 작업을 지연시키거나 시간의 경과를 측정하는 것이 Clock의 주요 기능이다.
extension Clock {
  func sleep(for: Self.Instant.Duration, tolerance: Self.Instant.Duration?) async throws
  func measure(() throws -> Void) rethrows -> Self.Instant.Duration
  func measure(() async throws -> Void) async rethrows -> Self.Instant.Duration
}
```

- [`ContinuousClock`](https://developer.apple.com/documentation/swift/continuousclock): 스톱 워치와 같이 측정 대상의 상태와 상관없이 시간을 증가시킨다. 즉 시스템이 절전 상태일 때에도 시간을 증가시키기 때문에 실제 시간(human time)을 측정하는데 적절하다.
- [`SuspendingClock`](https://developer.apple.com/documentation/swift/suspendingclock): 시스템이 절전 상태일 때 중지되서 device time을 측정하는데 적절하다. 예를 들어, 아래 코드를 실행하고 맥북을 잠자기 상태에 두면 `ContinuousClock`은 계속해서 시간을 증가시킨다. 반면에 `SuspendingClock`은 맥북이 잠자기 상태일 동안 중지되고, 다시 깨웠을 때부터 시간을 증가시킨다.

  ```swift
  let suspendingClock = SuspendingClock()
  let suspendingClockElapsed = await suspendingClock.measure {
    await someLongRunningWork()
  }

  let continuousClock = ContinuousClock()
  let continuousClockElapsed = await continuousClock.measure {
    await someLongRunningWork()
  }
  ```

&nbsp;
### Instant

시간의 한 지점을 나타낸다.

```swift
public protocol InstantProtocol: Comparable, Hashable, Sendable {
  associatedtype Duration: DurationProtocol

  // 현재 지점에서 `duration` 만큼 경과한 시간을 반환하는 함수가 있다.
  func advanced(by duration: Duration) -> Self

  // 두 instant 사이에 경과된 시간을 반환하는 함수가 있다.
  func duration(to other: Self) -> Duration
}

// 두 instant를 더하거나 뺄 수 있다.
extension InstantProtocol {
  public static func + (_ lhs: Self, _ rhs: Duration) -> Self
  public static func - (_ lhs: Self, _ rhs: Duration) -> Self

  public static func +- (_ lhs: inout Self, _ rhs: Duration) -> Self
  public static func -= (_ lhs: inout Self, _ rhs: Duration) -> Self

  public static func - (_ lhs: Self, _ rhs: Self) -> Duration
}
```

&nbsp;
### Duration

두 instant 사이에 경과된 시간을 높은 정밀도로 나타내며, `Duration`을 사용하는 계산은 음수 값에서 양수 값까지 확장될 수 있다. 

```swift
public protocol DurationProtocol: Comparable, AdditiveArithmetic, Sendabe {
  static func / (_ lhs: Self, _ rhs: Int) -> Self
  static func /= (_ lhs: inout Self, _ rhs: Int)
  static func * (_ lhs: Self, _ rhs: Int) -> Self
  static func *= (_ lhs: inout Self, _ rhs: Int)
  static func / (_ lhs: Self, _ rhs: Self) -> Double
}
```

`Duration`은 정의된 특정 시간 값에 대한 정적 메서드를 통해 생성되어야 한다.

```swift
extension Duration {
  public static func seconds<T: BinaryInteger>(_ seconds: T) -> Duration
  public static func seconds(_ seconds: Double) -> Duration
  public static func milliseconds<T: BinaryInteger>(_ milliseconds: T) -> Duration
  public static func milliseconds(_ milliseconds: Double) -> Duration
  public static func microseconds<T: BinaryInteger>(_ microseconds: T) -> Duration
  public static func microseconds(_ microseconds: Double) -> Duration
  public static func nanoseconds<T: BinaryInteger>(_ nanoseconds: T) -> Duration
}
```

&nbsp;
### Task

`Clock`이 추가되면서 [`Task`](https://developer.apple.com/documentation/swift/task) API가 변경됐다. 기존 `Task` API의 sleep 메서드가 deprecated 되고 Clock이 적용된 메서드가 추가된다.

```swift
extension Task {
  @available(*, deprecated, renamed: "Task.sleep(for:)")
  public static func sleep(_ duration: UInt64) async

  @avilable(*, deprecated, renamed: "Task.sleep(for:)")
  public static func sleep(nanoseconds duration: UInt64) async throws

  public static func sleep(for: Duration) async throws

  public static func sleep<C: Clock>(until duration: C.Instant, tolerance: C.Instant.Duration? = nil, clock: C) async throws
}
```

새로운 메서드는 나노초 단위보다 정밀하게 표현할 수 있다.

```swift
try await Task.sleep(until: .now() + .seconds(1), clock: .continuous)
```

또한 `tolerance`를 통해 버퍼를 설정해서 성능을 극대화할 수 있다.

```swift
try await Task.sleep(until: .now() + .seconds(1), tolerance: .seconds(0.5), clock: .continuous)
```

&nbsp;
## 참고 문헌
- [Hacking with Swift / What's new in Swift 5.7](https://www.hackingwithswift.com/articles/249/whats-new-in-swift-5-7)
- [Swift Evolution / SE-0329 Clock, Instant, and Duration](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md)
