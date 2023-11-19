# State
### SwiftUI에서 관리되는 값을 읽고 쓸 수 있는 프로퍼티 래퍼 유형입니다.

```Swift
@frozen @propertyWrapper
struct State<Value>
```

---

## OverView
value 타입을 뷰 계층구조에 저장할 때, 해당 값의 최신 및 정확한 버전을 유지하기 위해 State를 사용합니다.  
프로퍼티 선언에 @State 속성을 적용하고 초기 값을 제공하여 App, Scene, View에서 상태값을 생성하세요.
SwiftUI에서 제공하는 storage management와 충돌할 수 있는 memberwise initializer에서 설정되는 것을 방지하기 위해서 State는 Private로 선언하세요.  

```Swift
struct PlayButton: View {
    @State private var isPlaying = false // Create the State
    
    var body: some View {
        Button(isPlaying ? "Pause" : "Play") { // Read the State
            isPlaying.toggle() // Write the State
        }
    }
}
```
SwiftUI는 프로퍼티의 스토리지를 관리합니다.  
값이 변경되면 SwiftUI는 해당 값에 의존하는 뷰 계층 구조의 일부를 업데이트 합니다.  
State의 기본 값에 액세스 하려면 WrappedValue 속성을 사용합니다.  
하지만 Swift를 사용하면 State 인스턴스를 직접 참조하여 WrappedValue에 접근할 수 있습니다.  
위의 예제에서는 isPlaying에서는 State의 Wrapped value에 직접 Read, Write 작업을 하는것을 볼 수 있습니다.  
<br/>
값에 액세스 해야하는 뷰 계층구조에서 가장 상위 뷰에서 State를 Private로 선언하세요.  
그런 다음 액세스가 필요한 하위 뷰들과 상태를 공유하세요.  
읽기 전용 액세스를 위해 직접 공유하거나 read-write 액세스를 위해 바인딩으로 공유할 수 있습니다.  
모든 스레드에서 State 프로퍼티들을 안전하게 변경할 수 있습니다.

<br/>


## 하위뷰(subview)들과 State 공유
State 프로퍼티를 subview로 전달하면 SwiftUI는 container view에서 값이 변경될때마다 subview를 업데이트 합니다.  
하지만 subview에서 값을 수정할 수는 없습니다.  
subview에서 상태의 저장된 값을 수정할 수 있도록 하려면 State 대신에 Binding을 전달해야 합니다.  

예를들어 위 예제의 PlayButton에서 isPlaying State를 제거하고 Binding으로 변경합니다.

```Swift
struct PlayButton: View {
    @Binding var isPlaying: Bool // Play Button now Receive a binding (default 값 있으면 오류발생.)
    
    var body: some View {
        Button(isPlaying ? "Pause" : "Play") {
            isPlaying.toggle()
        }
    }
}
```
그런 다음 State를 선언하고 State에 대한 Binding을 생성하는 Player view를 정의할 수 있습니다.  
State의 projectedValue에 액세스하여 State 값에 대한 바인딩을 가져옵니다. 이 바인딩은 속성 이름 앞에 $ 기호를 붙이면 얻을 수 있습니다.

```Swift
struct PLayuerView: View {
    @State private var isPlaying: Bool = false // Create the state here now
    
    var body: some View {
        VStack {
            PlayButton(isPlaying: $isPlaying) // Pass a binding
            
            // ...
        }
    }
}
```
StateObject를 선언할 때 처럼, memberwise initializer에서 설정되는것을 방지하기 위해 State를 private로 선언하세요.  
이것은 SwiftUI가 제공하는 storage management와 충돌할 수 있습니다.  
StateObject 다르게 위의 예제처럼 State 객체를 선언할 때에는 기본값을 제공하여 초기화하세요.
State는 view, subview에 로컬로 사용되는 저장 용도로만 사용하세요.

<br/>

## Store observable objects (Observable 객체 저장하기)  
Observable() 매크로를 사용하여 생성한 observable objects를 State에 저장할 수 있습니다.   
<b>여기서부터는 다음에 @Observable를 공부하고 계속 작성할 예정</b>


--- 
### 생긴 궁금증
- StateObject에 대해서 알아보자.
- SwiftUI에서 제공하는 storage management는 무엇이고 어떻게 동작하지?  
- Observable Object에 대해서 알아보자.
- @frozen은 무엇일까?

---
- 출처 https://developer.apple.com/documentation/swiftui/state
