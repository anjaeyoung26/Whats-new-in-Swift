## Retrieve UIImages by locale

iOS 17부터 심볼 이미지의 locale을 설정하는 기능이 추가된다.

|코드|심볼 이미지|
|---|---|
|<pre>imageView.image = UIImage(<br>  systemName: "character.textbox"<br>)</pre>|<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/6eb1dc40-6105-41d6-a93e-d4a900da9aa8" width="200">|
|<pre>let locale = Locale(languageCode: .japanese)<br><br>imageView.image = UIImage(<br>  systemName: "character.textbox",<br>  withConfiguration: UIImage.SymbolConfigration(locale: locale)<br>)|<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/04256eaf-e978-40e4-b0b1-6233214e38fb" width="200">|
