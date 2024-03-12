# Published
### Property marked와 attribute를 게시하는 타입입니다.  A type that publishes a property marked with an attribute.
 
```Swift
@propertyWrapper
struct Published<Value>
```

<br/>

---

<br/>

## Overview
@Published 를 사용하여 프로퍼티를 생성하면 Publisher 타입이 생성됩니다.
해당 퍼블리셔에 접근하려면 $연산자를 사용합니다.

```Swift
class Weather {
    @Published var temperature: Double

    init(temperature: Double) {
        self.temperature = temperature
    }
}

// ...

let weather = Weather(temperature: 30)
let cancellable = weather.$temperature.sink { temperature in
    print("Sink Temperature : \(temperature)")
    print("Weather Temperature : \(weather.temperature)")
    print("\n")
}

weather.temperature = 10
```

```
Print Output
Sink Temperature : 30.0
Now Weather Temperature : 30.0

Sink Temperature : 10.0
Now Weather Temperature : 30.0
```

프로퍼티가 변경되면 프로퍼티의 willSet 블록에서 게시(pulibshing)이 발생하며, 이는 구독자들이 실제로 프로퍼티에 설정되기 전에 새로운 값을 받게됩니다.  

위의 예시에서, sink가 두번째 클로저를 실행할 떄 파라미터 10을 받게됩니다.  
하지만 클로저에서 weather.temperature를 확인한다면, 반환되는 값은 30이 됩니다.

<br/>

> ### Important
> @Published 속성은 클래스에 제한되어 있습니다. struct과 같은 클래스가 아닌 타입의 속성으로 사용하지 마세요.   
>  .