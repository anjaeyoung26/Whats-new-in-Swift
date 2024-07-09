## Represent fractional page progress

`UIPageCotrol`에서 정해진 시간동안 슬라이드 쇼를 보여줄 수 있는 기능이 추가됐다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/5091cf9b-02f6-4503-a4bf-0a93a58655e6" height="350">
</p>

아래와 같이 duration을 10초로 설정하면 10초마다 다음 페이지로 넘어간다. 또한 특정 상황에 타이머를 중지시킬 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/9b9ecbb3-2a7e-4497-ba03-5ddb16ebcc9e" width="700">
</p>

슬라이드 쇼는 사진 뿐만 아니라 동영상도 포함할 수 있다. `UIPageControlProgress`로 동영상의 재생 시간을 나타내는데 적절하다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/f5ee16fc-3d44-4a60-9ebd-8edead6426c7" width="700">
</p>

만약 슬라이드 쇼에 사진과 동영상이 모두 포함되어 있다면 `UIPageControlTimerProgress`와 `UIPageControlProgress`를 혼용한다.

```swift
@IBAction func pageChanged(_ sender: UIPageControl) {
  switch sender.currentPage {
  case 0: sender.progress = progress
  default: sender.progress = timerProgress
  }
}
```
