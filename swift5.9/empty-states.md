## Empty states

사용자가 검색한 키워드에 대한 결과가 없거나, 좋아요를 누른 목록이 없을 때와 같이 화면에 나타낼 컨텐츠가 없는 상태를 empty state 라고 표현한다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/95c6a926-8e51-4d95-b2e7-31f486d74964" height="500">
</p>

지금까지 empty state를 나타내기 위해 뷰를 커스텀 해왔다. UIKit에는 empty state 화면을 편리하게 구성할 수 있는 기능이 추가됐다. 해당 기능은 iOS 17+부터 사용할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/a9370296-f49d-434d-8b34-923a2ef7aaac" width="700">
</p>

[`UIContentUnavailableConfiguration`](https://developer.apple.com/documentation/uikit/uicontentunavailableconfigurationstate)은 empty state 화면의 컨텐츠를 설정할 수 있다. 이미지, 텍스트 뿐만 아니라 보조 텍스트, 버튼, 정렬, 여백 등 다양한 프로퍼티를 통해서 empty state 화면을 구성할 수 있다. 또한 세 가지 상태에 따라 화면을 달리 구성할 수 있다.

```swift
var emptyConfig = UIContentUnavailableConfiguration.empty() // 컨텐츠 없음
var loadingConfig = UIContentUnavailableConfiguration.loading() // 로딩 중
var searchConfig = UIContentUnavailableConfiguration.search() // 검색 결과 없음
```

`UIViewController` 클래스는 전달된 상태에 따라 content-unavailable 환경을 업데이트 하는 메서드가 추가됐다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/af0a5854-9b56-454f-8de2-566f0546db1a" width="700">
</p>

[`contentUnavailableConfigurationState`](https://developer.apple.com/documentation/uikit/uiviewcontroller/4176653-contentunavailableconfigurations) 프로퍼티의 값을 변경해서 원하는 상태로 업데이트 할 수 있다.

```swift
contentUnavailableConfigurationState = .empty()
setNeedsUpdateContentUnavailableConfiguration()
```
