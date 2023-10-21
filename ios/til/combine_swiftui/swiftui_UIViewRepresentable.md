# UIViewRepresentable - SwiftUI 에서 UIKit 사용하기


SwiftUI에서 UIView같은 UIKit을 사용할 수 없다.

Button을 자세히 보면 아래의 형태와 같다. UIButton으로 만든게 아니다.

``` Swift
public struct Button<Label> : View where Label : View { ... }
```

하지만 우리는 UIKit을 사용해야 될 때가 있다.

그럴때 사용하는것이 **UIViewRepresentable** 이다.

UIView를 SwiftUI 컴포넌트(?)로 변환할 수 있게 해주는 **프로토콜** 이다.



## 사용법



### 참고문서
- https://developer.apple.com/documentation/swiftui/uiviewrepresentable
- https://ios-development.tistory.com/1043
- https://code-algo.tistory.com/91

