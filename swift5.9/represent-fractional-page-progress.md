## Represent fractional page progress

`UIPageCotrol`에서 정해진 duration 동안 슬라이드 쇼를 보여줄 수 있는 기능이 추가됐다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/5091cf9b-02f6-4503-a4bf-0a93a58655e6" height="350">
</p>

아래와 같이 `UIPageControlTimerProgress`를 사용해서 duration을 10초로 설정한다면, 10초마다 자동으로 다음 페이지를 보여준다. 그리고 특정 상황에서 타이머를 중지하고 싶다면 `resumeTimer`를 호출한다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/9b9ecbb3-2a7e-4497-ba03-5ddb16ebcc9e" width="700">
</p>

슬라이드 쇼는 사진 뿐만 아니라 동영상도 컨텐츠에 포함할 수 있다. `UIPageControlProgress`는 현재 progress를 직접 설정할 수 있어서, 동영상의 재생 시간을 나타내는데 적절하다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/f5ee16fc-3d44-4a60-9ebd-8edead6426c7" width="700">
</p>

만약 슬라이드 쇼의 컨텐츠에 사진과 동영상이 모두 포함되어 있다면, `UIPageControlTimerProgress`와 `UIPageControlProgress`를 혼용한다.

```swift
@IBAction func pageChanged(_ sender: UIPageControl) {
  switch sender.currentPage {
  case 0: sender.progress = progress
  default: sender.progress = timerProgress
  }
}
```