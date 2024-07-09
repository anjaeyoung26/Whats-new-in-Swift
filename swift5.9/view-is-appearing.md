## viewIsAppearing

뷰 컨트롤러의 lifecycle에 [viewIsAppearing](https://developer.apple.com/documentation/uikit/uiviewcontroller/4195485-viewisappearing)이 추가된다. 이는 `viewWillAppear`과 `viewDidAppear` 사이에 추가되는데 뷰가 화면에 보이기 직전 UI를 업데이트하기 적절한 함수이다. 왜냐하면 `viewWillAppear`는 하위 뷰들이 추가되기 시작하는 시점이므로 뷰의 크기나 위치를 정하기에 너무 빠르다. 또한 `viewDidAppear`는 이미 뷰가 화면에 보이는 시점이므로 사용자에게 뷰가 업데이트되는 동작이 보이기 때문에 좋지 않은 사용자 경험을 줄 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/fd8efdd8-130c-42f4-a2f8-721c3f0ff57f" height="500">
</p>

시스템은 뷰가 `layoutSubviews()`를 실행할 때마다 `viewWillLayoutSubviews` 및 `viewDidLayoutSubviews`와 같은 레이아웃 메서드를 호출한다. 이는 뷰가 transition 하거나 화면에 표시되는 동안 언제든지 발생할 수 있다. 이와 다르게 `viewIsAppearing`은 lifecycle 동안 한 번만 호출된다.

&nbsp;
## vs viewWillAppear

[공식 문서](https://developer.apple.com/documentation/uikit/uiviewcontroller/4195485-viewisappearing)는 아래와 같은 경우 `viewWillAppear`을 사용하고 그 외에는 `viewIsAppearing`을 사용하는게 적절하다고 한다.

- 애니메이션과 함께 추가할 [transitionCoordinator](https://developer.apple.com/documentation/uikit/uiviewcontroller/1619294-transitioncoordinator)에 접근할 때처럼 뷰의 transition이 시작되기 전에 콜백이 필요할 때
- 뷰 컨트롤러나 뷰의 traits, hierarchy, geometry에 의존하지 않는 작업을 수행할 때

|State at callback time|viewWillAppear|viewIsAppearing|
|------|---|---|
|Transition coordinator available for adding alongside animations|✓|✘|
|View added to hierachy|✘|✓|
|View controller and view trait collections updated|✘|✓|
|View geometry (size, safe area, and so forth) is accurate|✘|✓|
