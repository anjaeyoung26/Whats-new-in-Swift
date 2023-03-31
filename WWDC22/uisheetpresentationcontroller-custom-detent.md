## UISheetPresentationController - Custom detent

[UISheetPresentationController](https://github.com/anjaeyoung26/TIL/blob/main/Swift/uisheetpresentationcontroller.md)는 iOS 15 버전부터 사용할 수 있는 시트 스타일의 컨트롤러이다. 기존에는 `Detent.medium()`, `Detent.large()` 두 가지 종류의 *Detent*만 존재해서 시트를 정해진 높이로만 사용할 수 있었다. iOS 16 버전부터 사용자 정의 *Detent*를 통해 원하는 높이를 갖는 시트를 사용할 수 있다. 아래의 메서드를 통해 사용자 정의 *Detent*를 생성한다.

```swift
class Detent : NSObject {
  static func custom(
    identifier: UISheetPresentationController.Detent.Identifier? = nil, 
    resolver: (_ context: UISheetPresentationControllerDetentResolutionContext) -> CGFloat?
  ) -> UISheetPresentationController.Detent
}
```

위 메서드의 매개변수가 어떤 것인지 살펴보자. 

&nbsp;
### UISheetPresentationController.Detent.Identifier

`large`, `medium`과 같이 각 *Detent*에 대한 식별자이다. 사용자 정의 *Detent*를 사용하기 위해, 이를 위한 식별자를 정의해야 한다.

```swift
extension UISheetPresentationController.Detent.Identifier {
  static let myCustom = UISheetPresentationCotroller.Detent.Identifier("myCustom")
}
```

&nbsp;
### UISheetPresentationControllerDetentResolutionContext

사용자 정의 *Detent*의 최대 높이를 설정하기 위한 컨텍스트다. 컨텍스트는 `maximumDetentValue`라는 프로퍼티를 갖고 있는데, 이는 `Detent.large()`와 동일한 높이를 의미한다.

```swift
sheet.detents = [
  .custom(identifier: .myCustom) { context in 
    context.maximumDetentValue * CGFloat(self.slider.value)
  }
]
```

위 코드는 0 ~ 1의 범위를 갖는 슬라이더의 값에 따라 `maximumDetentValue`를 설정한다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/176196378-1c7035c1-dbb7-4992-bbfc-9e16261ce4bc.gif" height="350">
</p>

슬라이더의 값에 따라 시트의 높이가 번경됨을 확인할 수 있다.

