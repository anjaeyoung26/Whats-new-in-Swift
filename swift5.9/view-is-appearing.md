## viewIsAppearing

뷰 컨트롤러의 라이프 사이클에 [viewIsAppearing](https://developer.apple.com/documentation/uikit/uiviewcontroller/4195485-viewisappearing)이 추가된다. 이는 `viewWillAppear`과 `viewDidAppear` 메서드 사이에 추가되는데, 뷰가 화면에 보이기 직전 UI를 업데이트 하기 적절한 함수이다. 왜냐하면 `viewWillAppear`는 하위 뷰들이 슈퍼 뷰에 추가되기 시작하는 시점이므로, 뷰의 크기나 위치를 정하기에 너무 빠른 시점이다. 또한 `viewDidAppear`는 이미 뷰가 화면에 보이는 시점이므로, 사용자가 뷰의 업데이트되는 동작을 보기 때문에 자연스럽지 않은 사용자 경험을 제공할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/fd8efdd8-130c-42f4-a2f8-721c3f0ff57f" height="500">
</p>

시스템은 뷰가 `layoutSubviews()`를 실행할 때마다 `viewWillLayoutSubviews` 및 `viewDidLayoutSubviews`와 같은 레이아웃 메서드를 호출한다. 이는 뷰가 transition 하거나 또는 화면에 표시되는 동안 언제든지 발생할 수 있다. 하지만 `viewIsAppearing`은 `viewWillLayoutSubviews`, `viewDidLayoutSubviews`와 다르게 `viewIsAppearing`은 뷰 컨트롤러의 라이프 사이클 동안 한 번만 호출된다.

|State at callback time|viewWillAppear|viewIsAppearing|
|------|---|---|
|Transition coordinator available for adding alongside animations|✓|✘|
|View added to hierachy|✘|✓|
|View controller and view trait collections updated|✘|✓|
|View geometry (size, safe area, and so forth) is accurate|✘|✓|

공식 문서는 `viewWillAppear`과 어떤 차이점이 있는지 이해할 수 있도록 비교해서 보여준다. 문서에서는 `viewWillAppear`을 애니메이션과 함께 추가할 수 있는 [transitionCoordinator](https://developer.apple.com/documentation/uikit/uiviewcontroller/1619294-transitioncoordinator)가 있으므로, 뷰 컨트롤러의 transition과 애니메이션을 동시에 수행하려고 할 때나 혹은 뷰 컨트롤러나 뷰의 trait, hierachy, geometry에 의존하지 않는 작업을 수행할 때 사용하라고 권장한다.