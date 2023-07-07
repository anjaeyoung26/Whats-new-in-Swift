# Macros

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/e3d49254-847a-42d8-aa00-e141d43e7d4c" width="400">
</p>

본 내용은 WWDC23의 [Write Swift macros](https://developer.apple.com/videos/play/wwdc2023/10166/)에서 확인할 수 있다. 매크로는 코드를 전처리 하는 방법으로, 소스 코드를 컴파일할 때 변환하므로 반복적인 코드를 직접 작성하지 않아도 된다. 컴파일하는 동안 Swift는 평소와 같이 코드를 빌드하기 전에 코드의 모든 매크로를 확장한다. 매크로는 단순한 문자열 대체가 아니라 type-safe 하므로 매크로가 작동할 데이터의 타입을 정확하게 알려줘야 한다. 만약 매크로를 확장할 때 매크로의 구현에서 오류가 발생하면 컴파일러는 이를 컴파일 오류로 처리한다. 본 글에서는 매크로의 개념, 종류 및 생성하는 방법에 대해서 알아보고, 매크로의 rule을 나타내는 attribute의 종류는 다음에 정리할 예정이다.

&nbsp;
## Freestanding Macros

Freestanding 매크로는 [SE-0397](https://github.com/apple/swift-evolution/blob/main/proposals/0397-freestanding-declaration-macros.md)에서 제안된 매크로의 한 종류로, 선언에 첨부되지 않고 자체적으로 나타난다. 따라서 소스 코드의 어느 곳에서든 독립적으로 사용할 수 있다. Freestanding 매크로를 호출하기 위해서는 매크로의 이름 앞에 # 기호를 붙이고, 괄호 안에 매크로에 대한 매개변수를 쓴다.

```swift
func myFunction() {
  print("Currently running \(#function)")
  #warning("Something's wrong")
}
```

Freestanding 매크로는 `#function` 처럼 값을 생성하거나 `#warning` 처럼 컴파일 타임에 작업을 수행할 수 있다. 그런데 `#function`이나 `#warning`은 Swift 5.9 이전 버전부터 사용하지 않았나라는 의문이 들 수 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/6c83fa4a-73cf-4a32-b8ae-aac4a68e8962" width="550">
</p>

이미 우리가 리터럴 표현식으로 사용하던 `#column`, `#dsohandle`, `#fileID`, `#filePath`, `#file`, `#function`, `#line`은 Swift 5.9 버전부터는 매크로로 구현되어 있다. 해당 내용은 [Swift 5.9 Documentation](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/)의 [Literal Expression](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/expressions/#Literal-Expression)에서 확인할 수 있다.

&nbsp;
## Attached Macros

Attached 매크로는 [SE-0389](https://github.com/apple/swift-evolution/blob/main/proposals/0389-attached-macros.md)에서 제안된 매크로의 한 종류로, 연결된 선언을 수정한다. 예를 들어, 어떤 타입에 attached 매크로를 연결하면 프로토콜을 채택하는 것과 같이 코드를 추가한다. Attached 매크로를 호출하기 위해서는 이름 앞에 @ 기호를 붙이고, 괄호 안에 매크로에 대한 매개변수를 쓴다. 공식 문서에서는 attached 매크로에 대한 예시로 `OptionSet`을 들었다. 아래는 Swift 5.9 이전 버전에서 `OptionSet`을 사용하는 코드다.

```swift
struct SundaeToppings: OptionSet {
  let rawValue: Int
  static let nuts = SundaeToppings(rawValue: 1 << 0)
  static let cherry = SundaeToppings(rawValue: 1 << 1)
  static let fudge = SundaeToppings(rawValue: 1 << 2)
}
```

위 코드에서는 각 옵션에 `rawValue`로 초기화하는 반복적이고 수동적인 이니셜라이저가 있다. 새 옵션을 추가할 때 값을 잘못 입력할 수 있다. `OptionSet` 프로토콜을 채택하는 대신 attached 매크로를 사용해서 해결할 수 있다.

```swift
@OptionSet<Int>
struct SundaeToppings {
  private enum Options: Int {
    case nuts
    case cherry
    case fudge
  }
}
```

위 매크로가 적용된 코드는 컴파일 타임에 아래와 같이 확장된다.

```swift
struct SundaeToppings {
  private enum Options: Int {
    case nuts
    case cherry
    case fudge
  }

  typealias RawValue = Int
  var rawValue: RawValue
  init() { self.rawValue = 0 }
  init(rawValue: RawValue) { self.rawValue = rawValue }
  static let nuts: Self = Self(rawValue: 1 << Options.nuts.rawValue )
  static let cherry: Self = Self(rawValue: 1 << Options.cherry.rawValue )
  static let fudge: Self = Self(rawValue: 1 << Options.fudge.rawValue )
}

extension SundaeToppings: OptionSet { }
```

&nbsp;
## 매크로 생성

Xcode -> [New file] -> [Swift Macro]를 클릭하면 프로젝트에 새로운 Swift 패키지가 생성된다. 패키지는 다음과 같은 파일들이 포함되어 있다.

<p align="center">
<img src="https://github.com/anjaeyoung26/GithubActions/assets/61190690/e3edc000-ecd0-4368-b264-9fd8c505799c">
</p>

1. 매크로의 기능을 공개적으로 노출하는 탐색 라이브러리
2. 매크로를 실행하고 테스트하기 위한 기본 실행 파일이 포함된 클라이언트 라이브러리
3. 모든 매크로의 구현 정보를 포함하는 컴파일러 플러그인
4. 매크로의 기능을 테스트하기 위한 테스트 타겟

이론적으로 탐색 라이브러리와 컴파일러 플러그인만 정의하면 된다. 클라이언트 라이브러리는 별도로 매크로를 테스트하는 데 도움이 될 수 있으므로 유지하는 것이 좋고, 매크로를 안정적으로 개발하고 싶다면 테스트 타겟에서 단위 테스트를 통해 매크로의 기능을 검증할 수 있다. 매크로는 일반적인 함수 및 타입과 다르게 선언부와 구현부가 나뉘어져 있는데 탐색 라이브러리에 선언부를 작성하고, 컴파일러 플러그인에 구현부를 작성한다.

&nbsp;
## 선언부

매크로의 선언부는 이름, 매개변수, 사용할 수 있는 위치 및 생성하는 코드의 종류가 포함된다. 매크로를 선언하는 코드는 해당 매크로를 사용하는 코드와 다른 모듈에 있기 때문에 항상 `public`으로 선언된다.

```swift
@freestanding(expression) // 1
public macro buildDate() -> String = // 2
    #externalMacro(module: "MyMacrosPlugin", type: "BuildDateMacro") // 3
```

**1라인**은 매크로를 호출할 수 있는 소스 코드의 위치 및 매크로가 생성할 수 있는 코드의 종류와 같은 role을 나타낸다. **2라인**에는 매크로의 이름과 매개변수를 지정하는데 이름은 `buildDate`이며 인수를 받지 않고 `String`을 반환한다. 또한 `buildDate` 매크로는 freestanding 매크로이므로 변수 및 함수의 이름과 같이 lower camel case를 사용한다. 반대로 attached 매크로는 구조체 및 클래스의 이름과 같이 lower camel case 이름을 갖는다. **3라인**은 [`externalMacro(module:type:)`](https://developer.apple.com/documentation/swift/externalmacro(module:type:))을 사용해서 매크로 구현부의 위치를 Swift에 알려준다.

&nbsp;
## 구현부

매크로를 구현하려면 매크로의 확장을 수행하는 타입과 매크로를 API로 노출하도록 선언하는 라이브러리를 만들어야 한다. 패키지의 컴파일러 플러그인 파일을 열어서 아래와 같은 구조체를 추가한다. 매크로의 선언부에서 Expression 매크로라고 선언했기 때문에 `ExpressionMacro` 프로토콜을 채택하고, 이를 준수하기 위해 `expansion` 함수를 추가한다. `expansion` 함수 안에 `BuildDateMacro` 매크로가 확장되면 추가될 코드를 작성한다.

```swift
public struct BuildDateMacro: ExpressionMacro {
  public static func expansion(
    of node: some FreestandingMacroExpansionSyntax,
    in context: some MacroExpansionContext
  ) -> ExprSyntax {
    let date = ISO8601DateFormatter().string(from: .now)
    return "\"\(raw: Date)\""
  }
}
```

이제 `BuildDateMacro`를 API로 노출시키는 플러그인을 구현한다. 이전의 `BuildDateMacro` 구조체 아래에 다음과 같은 코드를 추가한다.

```swift
@main
struct MyMacrosPlugin: CompilerPlugin {
  let providingMacros: [Macro.Type] = [
    BuildDateMacro.self
  ]
}
```

매크로를 Package.swift의 타겟 목록에 추가한다.

```
.macro(
  name: "MyMacrosPlugin",
  dependencies: [
    .product(name: "SwiftSyntax", package: "swift-syntax"),
    .product(name: "SwiftSyntaxMacros", package: "swift-syntax"),
    .product(name: "SwiftCompilerPlugin", package: "swift-syntax")
  ]
),
```

`BuildDateMacro`는 Expression 매크로이므로, 클라이언트에서 변수 및 함수와 같이 사용할 수 있다.

```swift
print(#buildDate)
```

&nbsp;
## 참고 문헌

- [The Swift Programming Language (5.9 beta) / Macros](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/macros/)
- [Better Programming / Use Swift Macros to Initialize a Structure](https://betterprogramming.pub/use-swift-macros-to-initialize-a-structure-516728c5fb49)
- [김종권의 iOS 앱 개발 알아가기 / Swift 매크로 개념](https://ios-development.tistory.com/1404)