## Variable Symbols

*SF Symbols*는 WWDC21에서 *Hierarchical*, *Palette* 렌더링을 지원하기 시작하여 심볼 이미지가 여러 가지 색상으로 표시될 수 있다. WWDC22는 0~1의 범위를 갖는 값을 설정하여 이미지의 색상을 단계적으로 표시할 수 있는 *Variable Symbols*를 소개했다. 자세한 내용은 [Adopt Variable Color in SFSymbols](https://developer.apple.com/videos/play/wwdc2022/10158/) 에서 살펴볼 수 있다.

```swift
@objc private func didChangeSliderValue(sender: UISlider) {
  symbolImageView.image = UIImage(systemName: "slowmo", variableValue: Double(sender.value))
}
```

위 코드는 0 ~ 1의 범위를 갖는 `UISlider`의 `value`를 *Variable* 값에 설정한다. `variableVaule`도 마찬가지로 0 ~ 1의 범위를 갖는다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/177230984-5a492b99-3c39-4a7c-a2eb-b1970940267b.gif" height="400">
</p>

*Variable*을 지원하는 심볼 이미지는 SF Symbols 앱의 '변수' 탭에서 확인할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/177231268-28fcd713-c197-46ab-9c98-dee6d0032041.png" height="400">
</p>