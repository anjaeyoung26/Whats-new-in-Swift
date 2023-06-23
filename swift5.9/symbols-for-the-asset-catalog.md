## Symbols for the asset catalog

Xcode 15 버전부터는 에셋 리소스에 대한 심볼을 자동으로 생성한다. 아래와 같이 Clouds 에셋에 구름 모양의 이미지를 추가하면

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/8b0ca6e5-b068-4723-b2ac-3d994b5c8b12">
</p>

Xcode가 자동으로 에셋의 이름에 맞춰 심볼을 생성해서 코드 완성으로 쉽게 사용할 수 있다.

```swift
UIImage(resource: .cloud)
```

만약 에셋의 이름이 `ios_cloud` 형태라면 Xcode가 카멜의 형태로 심볼을 생성한다.

```swift
UIImage(resource: .iosCloud)
```

만약 리소스의 이름을 변경하면 심볼도 마찬가지로 변경되므로, 기존의 코드는 컴파일 에러가 발생한다.

```swift
UIImage(resource: .iosCloud) // Type 'ImageResource' has no member 'iosCloud'
```
