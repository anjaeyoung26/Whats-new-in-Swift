## #Preview macro

Xcode 14까지 preview는 SwiftUI의 `View`를 변경할 때 미리보기를 표시하는 기능이다. 그리고 UIKit에서 사용하기 위해서는 `UIViewRepresentable` 혹은 `UIViewControllerRepresentable` 프로토콜을 채택해서 별도의 타입을 정의해야 하는 번거로움이 있었다. 하지만 WWDC23에서 발표된 Swift 5.9의 매크로 기능 중 `#Preview`로 해결할 수 있다. 이는 preview 기능을 SwiftUI 뿐만 아니라 UIKit 및 AppKit에도 편리하게 사용할 수 있는 매크로다. 자세한 내용은 WWDC23의 [Build programmatic UI with Xcode Previews](https://developer.apple.com/videos/play/wwdc2023/10)에서 확인할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/1a3b0fb9-385b-432d-813f-4fe96d3027ce" width="700">
</p>

`UIViewController` 뿐만 아니라 `UIView` 클래스에도 사용할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/7baac23e-46c6-4d2f-b431-ff7b5783202b" width="700">
</p>
