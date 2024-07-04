## Unavailability condition

[SE-0290](https://github.com/appl/swift-evolution/blob/main/proposals/0290-negative-availability.md)는 `#available`의 반전된 형태인 `#unavailable`을 도입했다. 따라서 다음과 같은 코드를 작성하는 것보다:

```swift
if #available(iOS 15, *) {} else {
  // Code to make iOS 14 and earlier work correctlry
}
```

`#unavailable`을 사용하여 다음과 같은 코드를 작성할 수 있다:

```swift
if #unavailable(iOS 15) {
  // Code to make iOS 14 and earlier work correctly
}
```

