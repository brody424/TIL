# Binding
### 원본데이터의 소유자로 소유하는 값에 읽기 및 쓰기 권한을 부여하는 Property Wrapper

```Swift
@frozen @propertyWrapper @dynamicMemberLookup
struct Binding<Value>
```
<br/>

---

<br/>

## Overview
Binding을 사용하여 데이터를 저장하는 프로퍼티와 데이터를 표시하고 변경하는 뷰 사이에 양방향 연결을 만듭니다.  
Binding은 데이터를 직접 저장하는 대신 다른 곳에 저장된 원본스트림에 연결합니다.  
예를들어 재생과 일시 중지 사이를 전환하는 버튼은 Binding 프로퍼티 래퍼를 사용하여 부모 뷰의 프로퍼티에 Binding을 만들 수 있습니다.

```Swift
struct PlayButton: View {
    @Binding var isPlaying: Bool // Create the State
    
    var body: some View {
        Button(isPlaying ? "Pause" : "Play") { // Read the State
            isPlaying.toggle() // Write the State
        }
    }
}
```

부모 뷰는 재생 상태를 저장할 프로퍼티를 선언하여, 이 프로퍼티 값의 원본임을 나타내기 위해 State 프로퍼티 래퍼를 사용해야 합니다.

``` Swift
struct PlayerView: View {
    @State private var isPlaying: Bool = false // Create the state here now
    
    var body: some View {
        VStack {
            PlayButton(isPlaying: $isPlaying) // Pass a binding
            
            // ...
        }
    }
}
```

PlayerView는 PlayButton을 초기화할 때 State 프로퍼티의 바인딩을 버튼의 Binding 프로퍼티에 전달합니다.  
Property Wrapped 값에 $접두사를 적용하면 proejcted Value가 반환되며, State 프로퍼티의 경우 값에 대한 Binding을 반환합니다.  
유저가 PlayButton을 탭할때 마다 PlayerView는 isPlaying상태를 업데이트 합니다.

### 출처
https://developer.apple.com/documentation/swiftui/binding

### 생긴 궁금증
- @dynamicMemberLookup은 무엇인가?
- State의 Proejcted Value가 Binding인데 그러면 Binding은 스트림을 인건가? Binding<Int>는 Int형 스트림인건지..?
