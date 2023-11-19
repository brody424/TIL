# @State, @Binding

SwiftUI를 사용해서 개발하다 보면 프퍼티에 @State, @Binding 같은 Property Wrapper가 있는데 어떤역할을 하는지 알아보자!

<br/>

## @State
- SwiftUI에서 관리하는 값을 읽고 쓸 수 있는 프로퍼티 래퍼 유형
- private 로 선언해야 한다.
- subview로 값을 전달했을 때 업데이트 불가능.
- view, subview에서만 사용해야한다.
- 값이 변경되면 뷰가 업데이트 됨
- @State 속성으로 프로퍼티의 초기값을 정했다면 다른값으로 재할당 불가능하며, @Binding변수를 통해서 변경 가능.
- view의 body에서 값을 변경할 수는 없음. 아래의 코드는 가능. 
    ```Swift
    Button("Test") {
        isPlaying.toggle()
    }
    ```

## @Binding
- private로 선언하면 안됨. (객체 생성하고 초기화할 때 에러발생.)
- 다른뷰에서 @State 프로퍼티의 변경을 하고 싶을 때 @Binding으로 선언하여 전달해야함.
- @State의 프로퍼티의 projectedValue $접두사를 붙여서 사용함.
- @State의 값을 변경할 때 사용됨.


<br/><br/>


공부한 출처
- https://developer.apple.com/documentation/swiftui/state
- https://developer.apple.com/documentation/swiftui/binding
- https://medium.com/harrythegreat/swiftui-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-5%ED%8E%B8-state-binding-observedobject-83c00c3317cb
- https://ios-development.tistory.com/1159
- https://velog.io/@marisol/SwiftUI-Published-vs-State  