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
- RxSwift에서 제공하는 Observable은 class이고 Combine의 Publiser은 Protocol 이다.  
    Combine은 Protocol로 이루어진게 많다고함.



## Subscriber, Publisher
RxSwift에서의 Observer, Oberservable은 아마 Subscriber, Publisher 이름으로 대체된거 같다.  
뭐 node.js 아키텍처 공부할 때 pub/sub 패턴이라고 공부했는데 이름을 직관적이게 만든건가 싶기도 ㅋㅋ
- Publisher
    - RxSwift에서의 Observable과 비슷한 역할
    - 값 방출
    - Publisher<Int, Never> 이런식으로 사용하는데 <Output, Failure> 타입을 방출. 받을대도 동일한 타입으로 받음
    - Just, Future 같은거 제공해주는데 찾아보면서 작업하면 될듯


### Sink, Assgin



sink, assign등 기본 Subscriber(Operator?) 을 제공
