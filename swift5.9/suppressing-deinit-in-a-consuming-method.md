## Suppressing deinit in a consuming method

`consuming`은 소유권을 가져왔으니 개체를 더 이상 사용하지 않을 때 deinit 할 책임을 갖는다. noncopyable 타입은 클래스와 같이 deinit을 제공할 수 있지만 `discard` 혹은 `consume self`를 사용해서 명시적으로 deinit을 트리거할 수 있다.
