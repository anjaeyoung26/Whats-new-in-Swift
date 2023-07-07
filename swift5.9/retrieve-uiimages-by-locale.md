## Retrieve UIImages by locale

|코드|심볼 이미지|
|---|---|
|<pre>imageView.image = UIImage(<br>  systemName: "character.textbox"<br>)</pre>|<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/6eb1dc40-6105-41d6-a93e-d4a900da9aa8" width="200">|
|<pre>let locale = Locale(languageCode: .japanese)<br><br>imageView.image = UIImage(<br>  systemName: "character.textbox",<br>  withConfiguration: UIImage.SymbolConfigration(locale: locale)<br>)|<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/04256eaf-e978-40e4-b0b1-6233214e38fb" width="200">|

Symbol 이미지에 적용할 글꼴, 크기, 스타일을 포함하는 [`UIImage.SymbolConfigration`](https://developer.apple.com/documentation/uikit/uiimage/symbolconfiguration)는 iOS 17 버전부터 [`Locale`](https://developer.apple.com/documentation/foundation/locale)을 설정할 수 있다.