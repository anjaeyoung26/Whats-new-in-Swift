## Internationalization

Internationalization 기능을 소개하기 전에 Font에 대한 용어를 정리하자.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/690a2dcf-9e92-4925-a5f5-12af589c7e0b" width="600">
</p>

- x-height: 소문자 위에 위치하는 임의의 선
- baseline: 모든 문자들이 공통적으로 깔고 앉는 선
- line-height: baseline 간의 간격
- Ascenders: x-height 위에 튀어나오는 문자의 부분
- Descenders: baseline 밑에 튀어나오는 문자의 부분

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/cc5bcf9a-50e2-40fa-944a-aa058ed1b3ae" width="600">
</p>

위와 같이 영어는 Ascenders와 Descenders가 적지만, 아랍어나 힌두어는 많이 두드러진다. 가장 오른쪽의 버마어는 영어의 line-height에 약 1.5배 정도로 각 나라의 언어에 따라 차이가 크다. 따라서 Ascenders와 Descenders가 다른 라인의 텍스트와 겹칠 수 있다. 따라서 `UILabel`, `UITextField` 등의 텍스트를 표시하는 컴포넌트들이 언어를 인식하여 다앙햔 구조를 갖도록 조정한다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/3333607d-4a63-45cc-84e9-be77668544e3" width="600">
</p>

`traitOverrides`와 `typesettingLanguage`라 낯선 프로퍼티로 언어를 설정할 수 있는데, 이는 WWDC23에서 소개한 UIKit의 새로운 trait system이다. `UIWindowScene`, `UIView`, `UIViewController`, `UIPresentationController`의 클래스에서 `traitOverrides` 프로퍼티를 통해 trait 계층의 데이터를 수정할 수 있다. 새로운 trait system은 iOS 17+부터 사용할 수 있으며, 자세한 내용은 [Unleash the UIKit trait system](https://developer.apple.com/videos/play/wwdc2023/10057)의 14:31 Applying overrides 챕터에서 확인할 수 있다.