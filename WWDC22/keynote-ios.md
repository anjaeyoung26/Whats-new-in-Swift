# iOS

WWDC22의 키노트에서 iOS 16이 소개되었다. 루머로 떠돌던 위젯 등을 포함한 잠금 화면의 커스터마이징 지원이 포함되었다. 하지만 한국에서 사용률이 적은 지갑, 지도, 메시지 앱의 개선 사항이 많아 눈에 띄는 항목이 많지 않다.

> Message 앱, 받아쓰기, 지도 앱과 관련된 내용은 제외하고 정리하였습니다.

&nbsp;
## 잠금 화면

iOS 16에 추가되는 신규 개인화 기능으로 새롭게 탈바꿈한 잠금 화면이 소개되었다. iOS 16의 잠금 화면은 개인적이고 아름답고 유용하게 꾸밀 수 있는 새로운 방법을 제공한다. 

- 잠금 화면에서 손가락을 꾹 누르면 잠금 화면을 편집할 수 있는 화면이 나타난다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172371293-c451b3c2-6edb-4adc-a8ab-8725f3b0ffa0.png" height="450">
</p>

- 잠금 화면의 전체적인 색상이나 날짜와 시간을 나타내는 폰트 등 스타일을 변경할 수 있다. 또한 스와이프 제스처로 잠금 화면의 스타일을 테마 별로 변경할 수 있으며, 여기에 사용자가 커스텀한 스타일을 추가할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172370949-67a2edbe-55b9-4f1e-9fe8-7a61eb6b7d64.png" height="450">
<img src="https://user-images.githubusercontent.com/61190690/172372486-74fad610-8a01-4dae-aea5-b251d0839511.gif" height="450">
</p>

- 잠금 화면에 위젯을 추가할 수 있다. 위젯에는 기본적으로 시계, 날씨, 캘린더 등을 제공한다. 그리고 잠금 화면은 이제 하나로 국한되지 않으니, 잠금 화면마다 위젯을 다르게 구성할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172373665-5aa4755c-641e-4565-aa1d-fa3a12145df3.gif" height="450">
</p>

- 잠금 화면의 알림 표시 방식이 변경되었다. 기존에는 알림이 쌓여있을 때 잠금 화면의 배경 이미지를 가렸지만, iOS 16 부터는 하단에 표시되거나 아예 숨길 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172374082-26ab0871-de25-411d-96f7-54c0f7828153.png" height="450">
</p>

- Live Activities를 통해 잠금 화면의 위젯에 실시간으로 정보를 업데이트할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172375015-54ae4630-f9ed-4bc9-9c08-48a13ea332ca.gif" width="400">
</p>


&nbsp;
## 집중 모드

작년에 소개된 집중 모드가 발전됐다. 이제 잠금 화면을 여러 개 생성하여 각 잠금 화면에 적절한 집중 모드를 매치할 수 있다. 또한 앱에서 Focus Filter API를 통해 '집중 모드 필터'를 적용하여 방해가 될 만한 콘텐츠를 필터링할 수 있다. 예를 들어 Safari에서 업무 집중 모드 필터를 키면 일에 연관된 탭만 보여준다. 뿐만 아니라 메시지 앱 속의 대화와 메일 앱 속의 계정, 캘린더의 이벤트를 필터링할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172376260-29cc73df-8bb8-430d-ac6c-ca4d45b88274.PNG" height="450">
</p>

&nbsp;
## iCloud 공유 사진 보관함

최대 6명의 사용자가 공동으로 참여할 수 있는 iCloud 보관함이다. 사용자는 개인 사진 보관함에 있는 사진을 공유할 수도 있고, 카메라 앱에 새롭게 도입된 토글을 사용해 공유 보관함으로 사진이 자동 전송되도록 설정할 수 있다. 가족 구성원이 iCloud 보관함에 참여하여 사진을 원활하게 공유할 수 있는 새로운 방법이다. 공유 보관함에서 사진을 추가, 편집, 삭제할 수 있는 권한은 모두에게 동일하게 주어지며, 편집하거나 수정된 내용은 즉시 동기화된다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172382069-f8137461-cb21-45f6-9a72-099726125487.jpg" height="450">
<img src="https://user-images.githubusercontent.com/61190690/172382076-0d37ea1a-43e4-4972-9f73-212cc74e8578.jpg" height="450">
<img src="https://user-images.githubusercontent.com/61190690/172382078-f2326bf5-e230-4716-a979-e47c94a65b99.jpg" height="450">
</p>

&nbsp;
## 라이브 텍스트

라이브 텍스트의 사진 검색 기능이 한국어를 지원한다. 러시아어가 사라지고 우크라이나어가 추가된 것도 눈에 띈다. 이제는 사진뿐만 아니라 Vimeo와 같은 앱의 영상에서도 텍스트를 추출할 수 있다. 또한 작년에 소개된 Visual Look Up(반려동물, 랜드마크, 식물 등을 인식하여 사진 속 물체에 관한 자세한 정보를 알려주는 기능)에 이어서 올해는 이미지를 심층적으로 이해하는 새로운 기능이 추가되었다. 사진 속 피사체에 손을 대고 꾹 누르면 배경에서 떼어 내어 메시지 앱과 같은 곳에 전송할 수 있다.

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172378382-60292432-a3bc-4af6-9f20-cc35efca099e.gif" height="450">
</p>

개발자는 Live Text API를 통해 라이브 텍스트의 기능을 앱에 적용할 수 있다.


&nbsp;
## 추가적으로..

<p align="center">
<img src="https://user-images.githubusercontent.com/61190690/172383837-4d9150c1-88ca-4ace-a6a0-f08bbda4482e.PNG" width="650">
</p>

iOS 16 버전과 호환되는 기기가 공개되었다. 오랫동안 버텼던 iPhone 6s가 사라졌다.