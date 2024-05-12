# Property Wrapper 공식문서 번역

https://docs.swift.org/swift-book/documentation/the-swift-programming-language/properties/#Property-Wrappers   
위의 사이트에 있는 Property Wrapper 부분만 번역한 부분입니다.


# Property Wrapper
property wrapper는 property가 저장되는 방법을 관리하는 코드와 property를 정의하는 코드 사이의 분리 계층을 추가합니다.  
예를들어, thread-safety 체크하거나 기본 데이터베이스에 저장하는 프로퍼티가 있는 경우 모든 프로퍼티에 해당 코드를 작성해야 합니다.  
property wrapper를 사용하여 코드를 한번만 작성하고 정의하면 중복 코드를 막고 재사용할 수 있습니다.  

property wrapper를 정의하려면 struct, class, enum으로 만든 후 wrappedValue를 정의하면 됩니다.  
아래의 코드를보면, TwelveOrLess struct의 wrappedValue는 항상 12 이하임을 보증합니다.   
만약 12 이상의 값을 저장해달라고 요청해도 12를 저장하게 됩니다.

```Swift
@propertyWrapper
struct TwelveOrLess {
    private var number = 0
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 12) }
    }
}
```

setter에서는 값이 12보다 작거나 같도록 하고, getter에서는 저장된 값을 반환합니다.

```
Note  
위의 예제에서 number에 대한 선언은 private으로 선언하여 TwelveOrLess struct 내부에서만 사용할 수 있게 해야합니다.  
다른곳에서 작성된 코드는 WrappedValue의 getter와 setter를 사용하여 값에 액세스해야 하며, number에 직접 접근하여 사용하면 안됩니다.  
```

프로퍼티 앞에 wrapper property의 이름을 사용하여 적용할 수 있습니다.  
다음은 TwelveOrLess을 사용하여 크기가 항상 12 이하인 사각형을 만드는 방법을 알아봅니다.

```Swift
struct SmallRectangle {
    @TwelveOrLess var height: Int
    @TwelveOrLess var width: Int
}


var rectangle = SmallRectangle()
print(rectangle.height)
// Prints "0"


rectangle.height = 10
print(rectangle.height)
// Prints "10"


rectangle.height = 24
print(rectangle.height)
// Prints "12"
```

SmallRectangle에서 사용되는 height, width는 TwelveOrLess wrapper property를 사용하고 있습니다.   
그리고 초기값은 각각 0이 됩니다.  
height에 10을 대입하면 유효한 값이므로 10이 저장되게 됩니다.  
하지만 24는 TwelveOrLess에서 허용하는 값보다 큽니다. 그래서 24를 저장한다면 24가 아닌 맥시멈 값인 12가 저장되게 됩니다.  

프로퍼티에 wrapper를 적용하면 컴파일러는 해당 wrapper에 대한 저장소를 제공하고 wrapper를 통한 프로퍼티에 접근을 제공하는 코드를 합성합니다.
(property wrapper가 래핑된 값의 저장을 담당하므로, 그에 대한 synthesized(합성된) 코드는 없습니다.)
특별한 프로퍼티 구문을 활용하지 않고도 property wrapper의 동작을 사용하는 코드를 작성할 수 있습니다.    
예를들어, 이전 코드에서 @TwelveOrLess를 프로퍼티로 사용하는 대신 명시적으로 SmallRectangle속성을 TwelveOrLess 구조체로 래핑하는 버전을 다음과 같이 작성할 수 있습니다.  

```Swift
struct SmallRectangle {
    private var _height = TwelveOrLess()
    private var _width = TwelveOrLess()
    var height: Int {
        get { return _height.wrappedValue }
        set { _height.wrappedValue = newValue }
    }
    var width: Int {
        get { return _width.wrappedValue }
        set { _width.wrappedValue = newValue }
    }
}
```
_height 와 _width 프로퍼티는 TwelveOrLess(property wrapper)의 인스턴스를 저장합니다.  

height와 width의 getter, setter 는 wrappedValue 프로퍼티에 접근을 래핑합니다.  
(이해가 잘 안되는데 _height 이렇게 안만들고 위위 코드처럼 사용하면 된다는뜻 인듯.)


# 래핑된 프로퍼티에 대한 초기값 설정

위의 예제 코드에서는 TwelveOrLess의 정의에서 number에 초기값을 지정함으로써 래핑된 프로퍼티의 초기값을 설정했습니다.  
이 property wrapper를 사용하는 코드는 TwelveOrLess에 의해 래핑된 속성에 대해 다른 초기값을 지정할 수 없습니다.  
예를들어, SmallRectangle의 정의에서 height나 width의 초기값을 지정할 수 없습니다.  
초기값 설정 또는 다른 사용자 정의를 지원하려면 property wrapper는 추가적인 이니셜라이저를 제공해야 합니다.  

다음은 SmallNumber구조체는 TwelveOrLess의 확장된 버전으로, 이니셜라이저와 maximum을 래핑된 코드입니다.

```Swift
@propertyWrapper
struct SmallNumber {
    private var maximum: Int
    private var number: Int


    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, maximum) }
    }


    init() {
        maximum = 12
        number = 0
    }
    init(wrappedValue: Int) {
        maximum = 12
        number = min(wrappedValue, maximum)
    }
    init(wrappedValue: Int, maximum: Int) {
        self.maximum = maximum
        number = min(wrappedValue, maximum)
    }
}
```

SmallNumber의 에는 3가지 이니셜라이저가 존재합니다.   
- init() 
- init(wrappedValue:) 
- init(wrappedValue:maximum:)    

예제에서는 이니셜라이저를 사용하여 래핑된 값과 최대값을 설정합니다.  
초기화 및 이니셜라이저 구문에 대한 자세한 내용은 초기화 문서를 참조하세요.


```Swift
struct ZeroRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int
}


var zeroRectangle = ZeroRectangle()
print(zeroRectangle.height, zeroRectangle.width)
// Prints "0 0"
```

height와 width를 래핑하는 SmallNumber의 인스턴스는 SmallNumber()를 호출하여 생성됩니다.   
그 이니셜라이저 내부의 코드는 기본값인 number = 0, maximum = 12를 사용하여 초기값들을 설정합니다.  
property wrapper는 여전히 모든 초기값을 제공합니다..(이전 예제에서 사용했던 TwelveOrLess 처럼)  
하지만 TwelveOrLess와는 다르게 여러가지 방법으로 초기화할 수 있게 지원합니다.  
속성의 초기값을 지정할 때, Swift는 init(wrappedValue:) 이니셜라이저를 사용하여 래퍼를 정의합니다.  
예를 들어 아래의 코드가 있습니다.  

```Swift
struct UnitRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber var width: Int = 1
}


var unitRectangle = UnitRectangle()
print(unitRectangle.height, unitRectangle.width)
// Prints "1 1"
```

래핑된 프로퍼티 값에 = 1 을 작성하면, init(wrappedValue:) 이니셜라이저가 호출되며 변환해줍니다.  
height와 width를 래핑하는 SmallNumber의 인스턴스는 SmallNumber(wrappedValue: 1)을 호출합니다.  
해당 이니셜라이저는 여기에서 지정된 래핑값과 기본 최대값인 12를 사용합니다.  

사용자 정의 속성 뒤에 괄호 안에 인자를 작성하면, Swift는 해당 인자를 받아들이는 이니셜라이저를 사용하여 래퍼를 설정해줍니다.  
예를 들어, 초기값과 최대값을 제공하는 경우 Swift는 init(wrappedValue:maximum:) 이니셜라이저를 사용합니다.


```Swift
struct NarrowRectangle {
    @SmallNumber(wrappedValue: 2, maximum: 5) var height: Int
    @SmallNumber(wrappedValue: 3, maximum: 4) var width: Int
}


var narrowRectangle = NarrowRectangle()
print(narrowRectangle.height, narrowRectangle.width)
// Prints "2 3"


narrowRectangle.height = 100
narrowRectangle.width = 100
print(narrowRectangle.height, narrowRectangle.width)
// Prints "5 4"
```

height를 래핑하는 SmallNumber의 인스턴스는 SmallNumber(wrappedValue: 2, maximum: 5)를 호출하여 생성되며, width는 SmallNumber(wrappedValue: 3, maximum: 4)를 호출하여 생성됩니다.  
property wrapper에 인자를 포함함으로써, 래퍼의 초기 상태를 설정하거나 래퍼가 생성 될 때 다른 옵션을 전달할 수 있습니다.  

이 구문은 속성 래퍼를 사용하는 가장 일반적인 방법입니다.  
속성에 필요한 인자를 제공하면, 그것들이 이니셜라이저로 전달됩니다.  

property wrapper에 인자를 포함할 때, 할당(assignment)을 사용하여 초기값을 지정할 수 있습니다.  
Swift는 할당(assignment)을 wrappedValue 인자처럼 처리하고, 포함된 인자를 받아들이는 이니셜라이저를 사용합니다.  예를들어  

```Swift
struct MixedRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber(maximum: 9) var width: Int = 2
}

var mixedRectangle = MixedRectangle()
print(mixedRectangle.height)
// Prints "1"

mixedRectangle.height = 20
print(mixedRectangle.height)
// Prints "12"
```


height를 래핑하는 SmallNumber의 인스턴스는 SmallNumber(wrappedValue: 1)를 호출하여 생성되며, 이는 기본 최대값 12를 사용합니다.

width를 래핑하는 인스턴스는 SmallNumber(wrappedValue: 2, maximum: 9)를 호출하여 생성됩니다.
(신기하네 개떡같이 써도 찰떡같이 변환해주는 느낌?)
