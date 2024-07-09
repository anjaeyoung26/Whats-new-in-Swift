## Empty states

empty state란 화면에 표시할 컨텐츠가 없는 상태를 의미한다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/95c6a926-8e51-4d95-b2e7-31f486d74964" height="500">
</p>

iOS 17+부터 empty state를 편리하게 구성할 수 있는 기능이 추가되어서 더 이상 커스텀 뷰를 사용하지 않아도 된다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/a9370296-f49d-434d-8b34-923a2ef7aaac" width="700">
</p>

위 `UIContentUnavailableConfiguration.empty()`는 기본적으로 제공하는 타입이며 추가적으로 `loading`, `search`도 있다. 각각 이미지, 텍스트, 버튼 등을 용도에 맞게 구성할 수 있다.

```swift
var emptyConfig = UIContentUnavailableConfiguration.empty()
var loadingConfig = UIContentUnavailableConfiguration.loading()
var searchConfig = UIContentUnavailableConfiguration.search()

searchConfig.text = "검색 결과 없음"
searchConfig.secondaryText = "Retry 버튼을 클릭해서 재시도 해주세요."

var buttonConfig =  UIButton.Configuration.filled()
buttonConfig.title = "Retry"
searchConfig.button = buttonConfig
```

`UIViewController` 클래스는 전달된 content-unavailable configuration으로 empty state 화면을 구성하는 기능이 추가된다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/af0a5854-9b56-454f-8de2-566f0546db1a" width="700">
</p>

[`contentUnavailableConfigurationState`](https://developer.apple.com/documentation/uikit/uiviewcontroller/4176653-contentunavailableconfigurations)의 값을 변경해서 원하는 상태로 변경할 수 있다.
