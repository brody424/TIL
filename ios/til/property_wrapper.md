# Property Wrapper

## 잡담  

SwiftUI 공부를 시작하는데 @State, @ObseredObject 같은게 보인다.

공부를 하기위해 제드님의 블로그를 보기 시작했다.

- https://zeddios.tistory.com/1221
- https://github.com/apple/swift-evolution/blob/main/proposals/0258-property-wrappers.md#introduction 

Swift 5.1에 추가된 기능인 Property Wrapper이라고 한다. (19년도에 추가된 기능이라니 ㄷㄷ..)


<br/>
<br/>

## 정리

- 로직의 중복을 막기위해 특별한 타입의 프로퍼티라고 알려주는 기능이다.
- 프로퍼티의 extension 느낌인듯 하다.
- 프로퍼티 앞에 @State 같이 붙인다. 
- 만들어서 사용할 수 있다.


<br/>
<br/>

## 코드

<img width="400" alt="스크린샷 2023-10-13 오후 9 03 54" src="https://github.com/brody424/TIL/assets/15370950/a8883fb5-7fdb-4f4c-8e7b-533df727a990">  

제드님 블로그를 보고 Upper 을 Lower로 바꿔보았다 ㅋㅋ.  
- 커스텀하게 만들 때는 @propertyWrapper를 쓰면 된다.  
- init은 있어도 되고 없어도 된다. (없으면 초기값이 없긴함.)
- 초기값을 넣기위해 init을 쓰려면 init(wrappedValue) 형태로 사용해야 된다. 
<br/><br/><br/>


<img width="400" alt="스크린샷 2023-10-13 오후 9 05 05" src="https://github.com/brody424/TIL/assets/15370950/2bccfdf5-0641-4dd0-bd7f-db963c3dc7cb">  

- init(wrappedValue) 형태로 안넣으면 오류 생김. LowerCase를 넣으라고함;;

<br/>

---
<br/>

## 마무리

가끔 중복되는 로직이 있으면 해당 기능을 사용하면 코드가 짧아지고 유지보수가 쉬울거 같다.