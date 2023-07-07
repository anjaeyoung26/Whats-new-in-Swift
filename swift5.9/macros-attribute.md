## Macros attribute

|Attribute|Description|
|---|---|
|@freestanding(expression)|Creates a piece of code that returns a value|
|@freestanding(declaration)|Creates one or more declaration|
|@attached(peer)|Adds new declarations alongside the declaration it's applied to|
|@attached(accessor)|Adds accessors to a property|
|@attached(memberAttribute)|Adds attributes to the declarations in the type/extension it's applied to|
|@attached(member)|Adds new declarations inside the type/extension it's applied to|
|@attached(conformance)|Adds conformances to the type/extension it's applied to|

Swift 5.9 버전에서 추가된 [Macros](./macros.md)는 선언부에 attribute를 붙일 수 있다. 이는 매크로의 rule을 나타내며, 크게는 [Freestanding Macros](./macros.md/#freestanding-macros)를 나타내는 `@freestanding`과 [Attached Macros](./macros.md/#attached-macros)를 나타내는 `@attached`가 있다. 그리고 뒤에 소괄호로 scope를 나타낸다.