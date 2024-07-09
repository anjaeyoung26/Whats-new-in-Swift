## Debug with structured logging

Xcode 15는 디버그 콘솔에서 [os_log](https://developer.apple.com/documentation/os/os_log)를 지원한다. Xcode 14까지는 별도의 콘솔 앱에서 로그를 확인할 수 있었지만, Xcode 15부터는 Xcode의 디버그 콘솔에서 확인할 수 있다. (os_log란 통합 로깅 시스템으로 모든 레벨의 시스템에서 메시징을 캡쳐할 수 있는 고성능 API이다. 개발자는 os_log를 이용해서 개발이나 운영 시 발생하는 문제점을 추적하거나 모니터링 할 수 있다.)

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/76f23ea2-87c1-4afb-a1e8-dc07d7a02257">
</p>

각 로그의 배경색은 레벨에 따라 다르게 표시되어 쉽게 분류할 수 있으며, 특정 레벨의 로그만 표시되도록 필터링할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/eed42c73-6623-4458-a913-27e3626461a0">
</p>

각 로그는 메타데이터를 가지고 있으며, 원하는 메타데이터만 볼 수 있도록 설정할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/05ecdb73-18ef-42f7-a013-b7b1f2dd21de">
</p>

로그에서 마우스 오른쪽 버튼 - Jump To Source를 클릭하면 해당 로그의 코드 라인으로 이동할 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/114ca6df-e7c3-4c3f-9188-11537184c715">
</p>

로그를 선택하고 스페이스바를 누르면 메타데이터를 포함한 정보가 표시된다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/0ea498de-0703-4487-962c-40d36d7dcf1a">
</p>
