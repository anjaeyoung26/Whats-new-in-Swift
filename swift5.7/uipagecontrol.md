## UIPageControl

iOS 16에서 `UIPageControl`의 새로운 기능을 알아보자. 자세한 내용은 WWDC22의 [What's new in UIKit](https://developer.apple.com/videos/play/wwdc2022/10068/)에서 살펴볼 수 있다.

### preferredCurrentPageIndicatorImage

iOS 14부터 `preferredIndicatorImage`를 통해 인디게이터의 이미지를 설정할 수 있었다. 이를 확장시켜 iOS 16부터는 현재 선택된 페이지를 나타내는 이미지를 별도로 설정할 수 있다:

```swift
pageControl.preferredCurrentPageIndicatorImage = UIImage(systemName: "star.fill")
pageControl.preferredIndicatorImage = UIImage(systemName: "star")
```

### Direction

iOS 16부터는 인디게이터의 배치 방향을 선택할 수 있다.

```swift
enum Direction: Int {
  case natural     // Locale에 따라 `leftToRight` 혹은 `rightToLeft`가 설정된다.
  case leftToRight // 첫 번째 인디게이터가 맨 왼쪽에, 마지막 인디게이터가 맨 오른쪽에 위치한다.
  case rightToLeft // `leftToRight`와 반대로 위치한다.
  case topToBottom // 첫 번째 인디게이터가 맨 윗쪽에, 마지막 인디게이터가 맨 아래쪽에 위치한다.
  case bottomToTop // `topToBottom`와 반대로 위차한다.
}
```

<p align="center">

|`leftToRight`|`rightToLeft`|`topToBottom`|`bottomToTop`|
|---|---|---|---|
|<img src="https://user-images.githubusercontent.com/61190690/175772626-d8ea35fd-7e23-4d1d-acbd-6ff5b53bbdd0.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772625-049bc7ba-fd16-45e9-a802-966e26921a2a.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772628-2fdbb236-1489-4e2b-8dc8-c264caaa6f00.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772627-7f246b44-d144-40b0-b130-b057090fd491.gif" height="400">|

</p>
