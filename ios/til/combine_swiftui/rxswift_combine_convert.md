# RxSwift -> Combine Converting

RxSwift를 사용해서 Combine의 용어와 사용법이 다르고 헷갈리는 경우가 종종있다.  

헷갈리거나 새로 알게된 부분 정리해보자!  

<br/>

- https://zeddios.tistory.com/925  
- https://sujinnaljin.medium.com/combine-combine-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-ac726ac40b07  
- https://developer.apple.com/documentation/combine 

위의 3개 참고하면서 공부함.

<br/>

---
<br/>

## RxSwift vs Combine
- RxSwift에서 제공하는 Observable은 class이고 Combine의 Publisher Protocol 이다.  
    Combine은 Protocol로 이루어진게 많다고함.


## Subscriber, Publisher
RxSwift에서의 Observer, Observable은 아마 Subscriber, Publisher 이름으로 대체된거 같다.  
뭐 node.js 아키텍처 공부할 때 pub/sub 패턴이라고 공부했는데 이름을 직관적이게 만든건가 싶기도 ㅋㅋ
- Publisher
    - RxSwift에서의 Observable과 비슷한 역할
    - 값 방출
    - Publisher<Int, Never> 이런식으로 사용하는데 <Output, Failure> 타입을 방출. 받을대도 동일한 타입으로 받음
        - Never, Error가 들어갈 수 있음
        - Never은 해당 스트림은 에러가 안난다고 선언해버림
        - sink할 때 Error면 sink(receiveCompletion: , receiveValue: ) 처럼 receiveCompletion의 사용을 강제함. (오류 난다고 선언 했는데 오류 나면 어케할껀데? 하는 느낌) 
    - Just, Future 같은거 제공해주는데 찾아보면서 작업하면 될듯
- Subscriber
    - RxSwift에서 옵저버 역할을 함.
    - Subscriber 클래스를 만들어서 인스턴스로 받을 수 있음
        - 해당 클래스에서 구독시작, 데이터, complete 등 이벤트를 모아서 처리 가능
        - 방출되는 이벤트 maximum을 정할 수 있음
    - sink로 간단하게 사용해도 됨(RxSwift에 .subscribe와 비슷)


## 스케쥴링

- RxSwift
    - SubscribeOn
        - 시작하는 Observable의 쓰레드를 변경해줌
        - 위치 상관 없음
    - ObserveOn
        - 아래의 Observable부터 쓰레드를 변경해줌

- Combine
    - SubscribeOn
        - RxSwift와 마찬가지로 Upstream의 쓰레드를 변경해줌
        - 위치 상관 없음
        - subscribeOn으로 글로벌로 변경하고 보내는 쪽에서 MainThread로 보내면 MainThread로 잡힘.
        (아마 global -> main으로 바꿔주는듯)
    - receiveOn
        - downStream의 쓰레드를 변경해줌

ObserveOn, receiveOn 이름을 헷갈리게 해놨구만..


## Cancellable
sink 메소드는 AnyCancellable(클래스) 를 반환함.   
RxSwift에서 subscribe가 disposable을 반환하는거랑 똑같은 개념으로 구독취소를 도와주는 녀석   

var cancellable = Set<AnyCancellable>()    
.store(&cancellable) 형식으로 사용함.   

RxSwift에서는 DisposeBag() 으로 만들면 끝이였는데   
Combine은 Set형식으로 만드는게 어색하네   

그리고 함수 이름도 .store보다 disposed(by)가 더 직관적인 느낌이 든다.   

## Operator
오퍼레이터는  상황에 맞게 검색해서 써야 될듯  
중복되는것도 많은데 지금 하나씩 공부하는건 좀...ㅋ




URLSession.shared.dataTaskPublisher(for: url) 처럼 사용할 수 있는데 확실히 편한듯.   
애플에서 만든 기능이라 Alamofire같은 서드파티 라이브러리 사용하지 않는게 매력적이긴 함.

