## Clock, Instant, and Duration

[SE-0329](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md)는 Swift에서 시간과 기간(duration)을 참조하는 새롭게 표준화된 방법을 제안한다.(iOS 16+) 이름에서 알 수 있듯이 세 가지 주요 구성 요소로 나뉜다.

&nbsp;
## Clock

`Clock`은 흘러간 시간을 측정하고 지정된 시점까지 작업을 지연시키는 매커니즘이다. 아래는 `Clock` 프로토콜의 선언부이다.

```swift
public protocol Clock: Sendable {
  associatedtype Duration: DurationProtocol
  associatedtype Instant: InstantProtocol where Instant.Duration == Duration

  var now: Instant { get }
  var minResolution: Instant.Duration { get }
  static var continuous: ContinuousClock
  static var suspending: SuspendingClock

  func sleep(until deadline: Instant, tolerance: Instant.Duration?) async throws
}

extension Clock {
  func sleep(for: Self.Instant.Duration, tolerance: Self.Instant.Duration?) async throws
  func measure(() throws -> Void) rethrows -> Self.Instant.Duration
  func measure(() async throws -> Void) async rethrows -> Self.Instant.Duration
}
```

현재 시간을 나타내며, `.now + .seconds(1)`과 같이 기준점으로 사용된다.

```swift
var now: Instant { get }
```

최소 시간 해상도를 나타내며, 기본 값은 `.nanosecond(1)`이다. 이는 1 나노초 단위로 시간 경과를 측정한다는 의미이다.

```swift
var minResolution: Instant.Duration { get }
```

지정된 시점까지 작업을 지연시키거나 시간의 경과를 측정하는 것이 Clock의 주요 기능이다.

```swift
func sleep(...)
func measure(...)
```

`Clock`은 [`ContinuousClock`](https://developer.apple.com/documentation/swift/continuousclock), [`SuspendingClock`](https://developer.apple.com/documentation/swift/suspendingclock) 두 가지가 내장되어 있다.

```swift
static var continuous: ContinuousClock
static var suspending: SuspendingClock
```

- `ContinuousClock`: 시스템이 절전 상태일 때에도 시간을 계속 증가시킨다. 사용자에게 파일을 내보낼 때 소요되는 시간 등을 측정하는데 사용할 수 있다.

  ```swift
  let clock = ContinuousClock()

  let time: ContinuousClock.Instant.Duration = clock.measure {
    // ...
  }

  print("Took \(time.component.second) seconds")
  ```

- `SuspendingClock`: `ContinuousClock`과 다르게 시스템이 절전 상태일 때 중지된다.

&nbsp;
## Instant

시간의 한 지점을 나타낸다. 아래는 `Instant` 프로토콜의 선언부이다.

```swift
public protocol InstantProtocol: Comparable, Hashable, Sendable {
  associatedtype Duration: DurationProtocol
  func advanced(by duration: Duration) -> Self
  func duration(to other: Self) -> Duration
}

extension InstantProtocol {
  public static func + (_ lhs: Self, _ rhs: Duration) -> Self
  public static func - (_ lhs: Self, _ rhs: Duration) -> Self

  public static func +- (_ lhs: inout Self, _ rhs: Duration) -> Self
  public static func -= (_ lhs: inout Self, _ rhs: Duration) -> Self

  public static func - (_ lhs: Self, _ rhs: Self) -> Duration
}
```

현재 지점에서 `duration` 만큼 경과한 시간을 반환하는 함수가 있다.

```swift
func advanced(by duration: Duration) -> Self
```

두 instant 사이에 경과된 시간을 반환하는 함수가 있다.

```swift
func duration(to other: Self) -> Duration
```

`Comparable`을 채택해서 시간을 더하고 빼는 연산자가 있다.

```swift
public static func + ...
public static func - ...
public static func +- ...
public static func -= ...
```

&nbsp;
## Duration

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
## Task

`Clock`이 추가되면서 [`Task`](https://developer.apple.com/documentation/swift/task) API가 업그레이드 됐다. 기존에 `Task` API에는 작업을 sleep하는 메서드가 있었지만, 기존의 메서드들은 deprecated 되고 Clock이 적용된 새로운 메서드가 추가된다.

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

새롭게 추가된 메서드는 나노 초 단위보다 정밀하게 표현할 수 있다.

```swift
try await Task.sleep(until: .now() + .seconds(1), clock: .continuous)
```

또한 `tolerance`를 통해 버퍼를 설정해서 성능을 극대화할 수 있다.

```swift
try await Task.sleep(until: .now() + .seconds(1), tolerance: .seconds(0.5), clock: .continuous)
```

위 코드에서 적어도 1초 안에 sleep 되는 것이 이상적이지만, 0.5초 정도의 버퍼를 허용해서 최대 1.5초 안에 sleep 되기를 기대한다.

&nbsp;
## 참고 문헌
- [Hacking with Swift / What's new in Swift 5.7](https://www.hackingwithswift.com/articles/249/whats-new-in-swift-5-7)
- [Swift Evolution / SE-0329 Clock, Instant, and Duration](https://github.com/apple/swift-evolution/blob/main/proposals/0329-clock-instant-duration.md)