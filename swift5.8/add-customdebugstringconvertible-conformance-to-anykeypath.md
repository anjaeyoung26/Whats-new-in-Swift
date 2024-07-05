## Add CustomDebugStringConvertible conformance to AnyKeyPath

Swift 5.8부터는 print 혹은 LLDB po로 `keypath`를 출력하는게 개선된다.

```swift
struct Theme {
  var backgroundColor: Color
  var foregroundColor: Color

  var overlay: Color {
    backgroundColor.withAlpha(0.8)
  }
}

print(\Theme.backgroundColor)
// ~ Swift 5.7: Swift.KeyPath<Theme, Color>
// Swift 5.8: "\Theme<offset 0 (Color)>"

print(\Theme.overlay)
// Swift 5.8: "\Theme.<computed 0xABCDEFG (Color)>"
```
