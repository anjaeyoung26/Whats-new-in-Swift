## Access-level modifiers on import declarations

[SE-0409](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0409-access-level-on-imports.md)는 import에서 접근 제어자를 사용할 수 있는 기능을 제안합니다.

```swift
// 명시할 수 있는 접근 제어자는 총 5가지이며 `open`은 사용할 수 없습니다.
public import PublicDependency
internal import InternalDependency
fileprivate imoport DependencyPrivateToThisFile
private import OhterDependencyPrivateToThisFile
package import PackageDependecy
```

이때 라이브러리의 기능을 동일한 접근 제어에 해당하는 곳에서만 사용할 수 있습니다.

```swift
internal import DatabaseAdapter

internal func internalFunc() -> DatabaseAdapter.Entry { ... } // O
public func publicFunc() -> DatabaseAdapter.Entry { ... } // error: function cannot be declared public because its result uses an internal type
```

그렇다면 Swift 6.0 이전의 import는 어떻게 될까요? 이들은 모두 기본적으로 `public`이 적용됩니다.

```swift
// ..<Swift 6.0
import DatabaseAdapter

// Swift 6.0
public import DatabaseAdapter
```