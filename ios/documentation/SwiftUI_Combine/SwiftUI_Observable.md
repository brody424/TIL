# Observable - Protocol
기존 데이터가 변경될 때 observers에게 알림을 보내는 형식입니다.

```Swift
protocol Observable
```

## Overview 
이 프로토콜을 준수하면 타입이 관찰을 지원한다는 것을 다른 API에 알립니다.  
하지만 Overvable 프로토콜 자체를 타입에 적용한다고 해서 해당 타입에 관찰기능이 추가되진 않습니다.  
대신 타입에 관찰 지원을 추가할 때 항상 Observable() 매크로를 사용하세요.


# Observable() - Macro
Observable 프로토콜의 적합성을 정의하고 구현하세요.

## Overview
해당 매크로는 사용자 지정 타입에 관찰 지원을 추가하고 타입을 Observable 프로토콜에 따릅니다.  
예를들어, 다음 코드는 Observable 매크로를 Car 타입에 적용하여 관찰 가능하게 합니다.

```Swift
@Observable 
class Car {
    var name: String = ""
    var needRepairs: Bool = false

    init(name: String, needRepairs: Bool = false) {
        self.name = name
        self.needRepairs = needRepairs
    }
}
```

## 요약.
타입에 값이 변경하면 관찰하는 Observable로 만들어 줌.  
RxSwift에서 사용하는 Observable과 이름이 똑같고 비슷한 역할을 하는거 같다.   

RxSwift에서의 Observable은 Stream을 만드는 방법을 여러가지 제공하지만  
애플에서 제공하는 Observable은 프로토콜과 매크로로 이루어져있으며 사용법이 다른것 같다.
