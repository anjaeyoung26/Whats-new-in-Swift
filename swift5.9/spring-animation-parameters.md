## Spring animation parameters

iOS 17부터 간단하게 스프링 애니메이션을 구현할 수 있는 [`UIView.animate(springDuration:bounce:...)`](https://developer.apple.com/documentation/uikit/uiview/4206320-animate) 메서드가 추가됐다.

```swift
@MainActor
class func animate(
  springDuration duration: TimeInterval = 0.5,
  bounce: CGFloat = 0.0,
  initialSpringVelocity: CGFloat = 0.0,
  delay: TimeInterval = 0.0,
  options: UIView.AnimationOptions = [],
  animations: () -> Void,
  completion: ((Bool) -> Void)? = nil
)
```

- `springDuration`: 스프링 애니메이션이 시작될 때까지의 시간
- `bounce`: 스프링 애니메이션이 실행되는 시간
- `initialSpringVelocity`:
- `delay`: 애니메이션을 시작하기 전에 대기하는 시간(초)
- `options`: 애니메이션을 수행할 방법을 나타내는 [`UIView.AnimationOptions`](https://developer.apple.com/documentation/uikit/uiview/animationoptions) 
- `animations`: 애니메이션의 duration 동안 동작할 애니메이션을 정의하는 코드 블록
- `completion`: 애니메이션이 완료된 후 수행할 동작을 정의하는 코드 블록

&nbsp;
## 예시

```swift
UIView.animate(springDuration: 0.5, bounce: 0.3) {
  circle.center.x += 100
}

// 모든 파라미터는 Optional이다.
UIView.animate {
  circle.center.x += 100
}
```

`bounce`를 0과 0.3으로 설정 했을 때를 비교하면 아래와 같다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/3a48ba91-0672-45f3-adff-7120ff7aee7f" width="700">
</p>

