# Property Wrapper의 Projecting Value

property wrapper는 래핑된 값 외에도 추가 기능을 노출하기 위해 projected value를 정의할 수 있습니다.  
예를들어, 데이터베이스 액세스를 관리하는 property wrapper는 projected value에 flushDatabaseConnection() 메서드를 노출시킬 수 있습니다.  
projected value는 래핑된 value의 이름돠 동일하며, projected value는 이름 앞에 달러기호($)를 사용합니다.   
코드에서는 $로 시작하는 프로퍼티를 정의할 수 없기 때문에, projected value는 사용자가 정의한 프로퍼티와 겹치지 않습니다.  

위의 SmallNumber 예제에서, number 값을 너무 큰 숫자로 설정하려고 하면 property wrapper가 해당 숫자를 저장하기 전에 조정했습니다.  

아래의 코드는 property wrapper가 새 값이 저장되기 전에 그 값을 조정했는지를 추적하기 위해 projectedValue를 프로퍼티를 추가했습니다.

```Swift
@propertyWrapper
struct SmallNumber {
    private var number: Int
    private(set) var projectedValue: Bool


    var wrappedValue: Int {
        get { return number }
        set {
            if newValue > 12 {
                number = 12
                projectedValue = true
            } else {
                number = newValue
                projectedValue = false
            }
        }
    }


    init() {
        self.number = 0
        self.projectedValue = false
    }
}
struct SomeStructure {
    @SmallNumber var someNumber: Int
}
var someStructure = SomeStructure()


someStructure.someNumber = 4
print(someStructure.$someNumber)
// Prints "false"


someStructure.someNumber = 55
print(someStructure.$someNumber)
// Prints "true"
```

someStructure.$someNumber를 사용하면 래퍼의 projected value에 접근할 수 있습니다.  
4와 같은 작은 숫자를 저장한 후에는 someStructure.$someNumber의 값이 false 입니다.  
하지만 55와 같이 큰 숫자를 저장한 후에는 projected value가 true로 변경되어 값을 조정했는지 확인할 수 있습니다.  

property wrapper의 projected value는 모든 유형의 값을 반환할 수 있습니다.  
이 예제에서 property wrapper는 숫자가 조정되었는지 여부와 같은 하나의 정보만 노출하므로, 해당 bool 값을 projected value 값으로 노출합니다.  
더 많은 정보를 노출해야 한다면 projected value값으로 다른 타입의 인스턴스를 반환하거나, 자신 자체를 반환할 수 있습니다.   

타입의 일부로서 코드에서 projected value 를 사용할 때(예를들어 프로퍼티의 게터와 인스턴스 메서드와 같이)  self 키워드를 생략할 수 있습니다.  
아래의 예제의 코드에서는 height와 width의 projected value값을 $height, $width로 참조합니다.  

```Swift
enum Size {
    case small, large
}


struct SizedRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int


    mutating func resize(to size: Size) -> Bool {
        switch size {
        case .small:
            height = 10
            width = 20
        case .large:
            height = 100
            width = 100
        }
        return $height || $width
    }
}
```

property wrapper 구문은 프로퍼티 getter, setter가 있는 속성일 뿐으로, height 와 width 에 액세스하는 것은 다른 속성에 액세스 하는것과 동일합니다.  

예를 들어, resize(to:)의 코드는 높이와 너비를 그들의 property wrapper를 사용하여 액세스 합니다.  
만약 resize(to: .large)를 호출하면, .large에 대한 switch case는 사각형의 높이와 너비를 100으로 설정합니다.  
래퍼는 이러한 속성들의 값이 12보다 크지 않도록 방지하고, 그들의 값이 조정되었다는 사실을 기록하이 위해 projected value값을 true로 설정합니다.  
resize(to:)의 끝에서, return 문은 $height, $width를 확인하여 property wrapper가 높이나 너비를 조정했는지를 확인합니다.  