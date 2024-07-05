## UISheetPresentationController - Custom detent

[UISheetPresentationController](https://github.com/anjaeyoung26/TIL/blob/main/Swift/uisheetpresentationcontroller.md)는 iOS 15에서 추가된 시트 스타일의 컨트롤러다. 기존에는 두 가지 Detent(medium, large)만 존재해서 시트를 정해진 높이로만 사용할 수 있었다. iOS 16부터는 사용자 정의 Detent로 원하는 높이를 설정할 수 있다.

```swift
class Detent : NSObject {
  static func custom(
    identifier: UISheetPresentationController.Detent.Identifier? = nil, 
    resolver: (_ context: UISheetPresentationControllerDetentResolutionContext) -> CGFloat?
  ) -> UISheetPresentationController.Detent
}
```

### UISheetPresentationController.Detent.Identifier

각 Detent의 식별자로, 사용자 정의 타입을 만들 때 정의해야 한다.

```swift
extension UISheetPresentationController.Detent.Identifier {
  static let myCustom = UISheetPresentationCotroller.Detent.Identifier("myCustom")
}
```

### UISheetPresentationControllerDetentResolutionContext

Detent의 최대 높이를 설정하는 것으로, `Detent.large()`와 동일한 높이인 `maximumDetentValue`라는 프로퍼티를 갖고있다.

```swift
sheet.detents = [
  .custom(identifier: .myCustom) { context in 
    context.maximumDetentValue * CGFloat(self.slider.value)
  }
]
```

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/176196378-1c7035c1-dbb7-4992-bbfc-9e16261ce4bc.gif" height="350">
</p>

