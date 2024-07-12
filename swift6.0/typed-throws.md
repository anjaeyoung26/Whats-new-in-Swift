## Typed throws

Swift 6.0 이전의 `throws`는 `Result`, `Task`에 비해 적은 정보를 전달합니다:

```swift
enum CatError: Error {
  case sleeps
  case sitsAtTree
}

func callCat() -> Result<Cat, CatError>
func callFutureCat() -> Task<Cat, CatError>
func callCatOrThrow() throws -> Cat
```

`Result`와 `Task`는 반환하는 에러의 타입을 명시할 수 있지만 `throws`는 불가능합니다. 따라서 `Result` 혹은 `Task`가 `throws`로 변환되면 타입에 대한 정보를 잃습니다:

```swift
func callAndFeedCat1() -> Result<Cat, CatError> 
  do {
    return Result.success(try callCatOrThrow())
  } catch {
    // callCatOrThrow가 CatError 타입의 에러를 반환하지 않기 때문에 컴파일 에러가 발생합니다.
    return Result.failure(error)
  }
}

func callAndFeedCat2() -> Result<Cat, CatError> {
  do {
    return Result.success(try callCatOrThrow())
  } catch let error as CatError {
    return Result.failure(error)
  } catch {
    // callAndFeedCat2는 CatError 타입의 에러를 반환하기 때문에 컴파일러가 완전성을 확인할 수 없어서 컴파일 에러가 발생합니다. 
    // 이때 에러가 catch 됐지만 반환할 수 없어서 모호해집니다.
    return Result.failure(error)
  }
}
```

이를 개선하고자 [SE-0413](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0413-typed-throws.md)은 특정 타입의 에러를 throw할 수 있는 기능을 제안합니다:

```swift
func callCat() throws(CatError) -> Cat {
  if Int.random(in: 0..<24) < 20 {
    throw .sleeps
  }
}
```

이러면 `throws`가 타입에 대한 정보를 가지고 있으므로 `Result`로 쉽게 변환할 수 있습니다:

```swift
func callAndFeedCat1() -> Result<Cat, CatError> {
  do {
    return Result.success(try callCat())
  } catch {
    // 에러가 `CatError` 타입이므로 컴파일 에러가 발생하지 않습니다.
    return Result.failure(error)
  }
}
```

### Throwing `any Error` or `Never`

아래의 `throws(any Error)`는 untyped throws와 동일합니다.

```swift
func throwsAnything() throws(any Error) { ... }
func throwsAnything() throws { ... }
```

또한 `throws(Never)`는 non-throwing와 동일합니다.

```swift
func throwsNothing() throws(Never) { ... }
func throwsNothing() { ... }
```