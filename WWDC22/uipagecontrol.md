## UIPageControl

iOS 16 버전에서 추가된 `UIPageControl`의 새로운 기능을 알아보자. 자세한 내용은 WWDC22의 [What's new in UIKit](https://developer.apple.com/videos/play/wwdc2022/10068/)에서 살펴볼 수 있다.

&nbsp;
### preferredCurrentPageIndicatorImage

iOS 14버전 부터 `preferredIndicatorImage`를 통해 인디게이터의 이미지를 설정할 수 있었다. 이를 확장시켜 iOS 16버전 부터는 현재 선택된 페이지를 나타내는 인디게이터에 또 다른 이미지를 설정할 수 있는 `preferredCurrentPageIndicatorImage` 프로퍼티가 추가되었다.

```swift
pageControl.preferredCurrentPageIndicatorImage = UIImage(systemName: "star.fill")
pageControl.preferredIndicatorImage = UIImage(systemName: "star")
```

기존처럼 `preferredIndicatorImage`만 설정하는 경우, 페이지가 선택됨에 따라 이미지의 알파가 변경되는 것과 같은 효과만 나타났다. 하지만 위와 같이 `preferredCurrentPageIndicatorImage`을 통해 페이지 컨트롤을 설정하면 선택된 인디게이터는 `star.fill` 이미지가, 선택되지 않은 인디게이터는 `star` 이미지가 나타난다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/175771769-6d0f43ee-5b9c-4737-9a15-0f25cbe490a4.gif" height="400">
</p>

&nbsp;
### Direction

페이지 컨트롤은 기본적으로 왼쪽부터 오른쪽 방향으로 인디게이터가 배치되어, 첫 번째 페이지를 나타내는 인디게이터가 페이지 컨트롤의 가장 왼쪽에 위치했다. iOS 16부터는 원하는 인디게이터 배치 방향을 선택할 수 있다. 배치 방향을 나타내는 `UIPageControl.Direction`은 총 다섯 가지가 존재한다.

```swift
enum Direction: Int {
  case natural
  case leftToRight
  case rightToLeft
  case topToBottom
  case bottomToTop
}
```

- `natural` : 이는 시스템의 *Locale*에서 쓰는 언어가 쓰이는 방향에 따라 `leftToRight` 혹은 `rightToLeft`가 설정된다.
- `leftToRight` : 첫 번째 페이지의 인디게이터가 맨 왼쪽에, 마지막 페이지의 인디게이터가 맨 오른쪽에 위치한다.
- `rightToLeft` : `leftToRight`와 반대로 첫 번째 페이지의 인디게이터가 맨 오른쪽에, 마지막 페이지의 인디게이터가 맨 왼쪽에 위치한다.
- `topToBottom` : 첫 번째 페이지의 인디게이터가 맨 윗쪽에, 마지막 페이지의 인디게이터가 맨 아래쪽에 위치한다.
- `bottomToTop` : `topToBottom`와 반대로 첫 번째 페이지의 인디게이터가 맨 아래쪽에, 마지막 페이지의 인디게이터가 맨 윗쪽에 위치한다.

  <p align="center">

  |`leftToRight`|`rightToLeft`|`topToBottom`|`bottomToTop`|
  |---|---|---|---|
  |<img src="https://user-images.githubusercontent.com/61190690/175772626-d8ea35fd-7e23-4d1d-acbd-6ff5b53bbdd0.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772625-049bc7ba-fd16-45e9-a802-966e26921a2a.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772628-2fdbb236-1489-4e2b-8dc8-c264caaa6f00.gif" height="400">|<img src="https://user-images.githubusercontent.com/61190690/175772627-7f246b44-d144-40b0-b130-b057090fd491.gif" height="400">|

  </p>