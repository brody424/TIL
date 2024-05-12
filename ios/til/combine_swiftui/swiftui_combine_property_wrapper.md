# SwiftUI, Combine 에서 사용되는 프로퍼티 래퍼를 비교해보자.

@State, @Binding, @StateObject, @ObservableObject, @ObservedObject, @Published, @EnvironmentObject 비교 ㄱㄱ   

헷갈려서 비교하는 표를 만들어서 확인하면 편할거 같아서 만들었다.



- @State와 @Binding 묶여서 사용되는 개념  
- @StateObject 와 @ObservedObject는 같이 사용됨.   
    - @State, @StateObject는 부모(?) 최초의 스트림(?) 원본(?) 을 제공  
    - @Binding, @ObservedObject는 위에서 받은 애들 사용  
    - 테스트해보니 ObservableObject class도 @State로 만들 수는 있지만 실시간 업데이트가 작동하지 않음.
- Published, ObservableObject 는 Combine에서 제공하는 기능  
- SwiftUI는 상태가 변경되면 뷰를 처음부터 다시 만들어서 그리는 동작이 있지만, @StateObject를 사용하면 뷰를 다시 만들지 않고 항상 동일한 뷰를 사용한다고 함.
    - @ObservedObject는 뷰가 다시 그려지며면서 초기화 되는데 @StateObject는 초기화 되지 않고 상태 유지함.(https://ios-development.tistory.com/1160 참고)
- @ObservedObject는 프로퍼티 래퍼고 ObservableObject는 프로토콜임.


추가로 iOS 17부터는 
Observable이 나왔으며 @Bindable 과 엮이고 @State에서 사용한다고함.
