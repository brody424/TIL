# SwiftUI, TCA로 개발하면서 느낀점

# 좋았던점

## 1. 재미있다
새로운 기술을 학습하는것도 재밌고 UI 짜는것도 직관적이여서 재밌다.    
물론 빡치는 순간이 훠어어얼씬 많지만 결국 만들어보면 재미가 있다.    
 
## 2. 버튼 touched 상태이 자동으로 되어있어서 UX적으로 좋다.
UIKit에서는 내가 직접 구현해줬어야 된거 같은데 자동으로 되어있어서 아주 굿.    

## 3. TCA action 해결하기 쉬운듯
Reducer를 분리해서 각각의 Subview를 그릴수 있고  
subview의 action은 부모뷰에서 처리하거나 subview에서 처리하는것을 선택할 수 있는것이 편한듯.  
cell의 이벤트를 부모뷰로 어떻게 보낼지 걱정 안해도 되서 좋다.   
그냥 action을 보내고 그 액션을 부모view에서 처리하면 되니까편함.   
근데 2중 처리가 될 수 있어서 조심하긴 해야될듯.   

## 4. View의 재사용은 확실히 편하다!
UIKit에서는 CustomView를 만들고 쓸 때 조금 불편했는데 SwiftUI는 그냥 가져다 쓰면 되서 편하다.   
만들때도 requred init 같은것도 필요없고 그냥 preview 보면서 만들면 되고 가져다 쓰는것도 매우 편하다.  
 

# 나빴던점

## 1. Preview + Xcode = Crash    
너무 자주 죽는다.   
역체감이 지린다고 Preview를 쓰면 편한데 Xcode 크래시 나는순간 스트레스 지수 200%!!   
그렇다고 Preview 없이 개발하면 넘나 불편..ㅠ  
조금 버벅이는 곳 프리뷰 작업했는데 1시간 10번 튕기는거 보고 프리뷰를 꺼버림ㅋㅋ    
로딩도 느리고 Xcode 크래시 나니까 그냥 빌드하는게 개발속도가 더 빠른 수준.

## 2. 없는 기능은 왜이렇게 많은지!!!
ScrollView + Paging 기능이 iOS17에서 쉽게 동작됨.  
하지만 iOS16 사용자가 11% 이기때문에 미니멈버전을 iOS16으로 잡음.    
헬게이트 오픈!  

```Swift
UIScrollView.appearance().isPagingEnabled = true
```
위의 코드를 사용하고 있지만 Current Index 가져오는 코드를 넣으면 페이징이 안됨 ㅂㄷㅂㄷ    
결국은 또 Custom UIKit을 써야 되는 건가..?   

## 3. iOS 17로 가야하는 것인가...?
Paging 안되서 UIKit으로 개발하려는데 TCA랑 엮이는것도 잘 안된다.   
@Bindable 쓰고 싶은데 iOS 17부터 쓸수 있는 모양이다.. ㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷ   
iOS 17부터 제공한다는 기능이 생각보다 많아서 개발하는 도중에 화가남!

## 4. TCA + SwiftUI 러닝커브가 생각보다 높다
TCA 버전이 안맞아서 에러가 나는지 iOS 버전이 안맞아서 에러가 나는지 찾기 너무 빡심.  
차라리 TCA를 버렸으면 이정도는 아니였을 수도..?  
격변하는 TCA, SwiftUI 중간에서 헤엄치는 나를 발견하고 헛웃음이 나왔다.  

## 5. 프리뷰 새로운 윈도우창에 못하나?
고정된 프리뷰를 새로운 윈도우창에 고정해놓고 싶다.  
프리뷰 사이즈를 고정해주던지!!!

## 6. Print는 왜 안됨..?
```Swift
let _ = print("AA")
```
이런식으로 쓰라고 하는데 굳이..?

## 7. 프리뷰 내가 원할 때만 리로드 못하나?
코드 하나 바꿨다고 계속 로딩되는데 내가 원할때만 리로드 하는 방법이 있음 좋을듯?

## 8. 생각보다 적은 레퍼런스들...
참고할게 생각보다 많지 않다.  
내가 검색을 잘 못하는 것일수도 있지만 자료가 많이 부족한 느낌.  
거기다가 계속 발전중이여서 스택오버플로의 답변에서 안되는게 많이 존재... GPT도 옛날 내용을 알려줘서 도움이 안됨...

## 9. 컴바인님...?
button.rx.tap 이 없어..?  
URLSession에는 사용하기 편하더니 왜 UIKit에는 없는거죠..?  
https://gist.github.com/d-date/735fa126e37ed88ff9d0011f6dc0510b   
위의 링크를 사용해서 rx처럼 쓰는데 흐음.. UIKit과 combine 브릿지가 필요하다니...   
RxCocoa님 그립읍니다 :(

## 10. Preview에서 @Environment(\.dismiss) var dismiss 가 있고 dismiss 호출하면 프리뷰크래시
Environment을 제공해주지 않는데 사용한다고 하니까 preview에서 크래시 발생하는 듯

## 11. 프리뷰 크래시 발생하면 찾기가 쉽지 않았다.
Firestore을 못쓰는건 이해하는데 크래시로그를 분석해서 찾아야 되는게 조금 불편..   
DocumentReference 는 프리뷰에서 못쓰는것으로 판단됨   
Optional이 아닌데 Optional로 바꾸고 개발하고 나중에 배포할 때 Optional 삭제해야 될듯?

## 12. AlertState Action에 ForEach가 안되는데 ㄷㄷ
불편해!   
ForEach 써서 반복적인 뷰를 쓰고 싶은데 쓸수가 없다.ㅠ

## 13. 뻐킹 sheet 속도 왜이럼   
클릭하면 1초 후에 sheet이 열린다.    
alert, present는 정상 동작하는데 sheet만 유독 이러는데 왜이러지   
SwiftUI @State로 sheet 띄우면 바로 뜨는데  
TCA를 사용해서 sheet 띄우면 1초정도 후에 뜨는 이슈 발견함 ㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷㅂㄷ  

## 14. 뻑하면 안나오는 힌트. 알수없는 빌드 오류
힌트가 뻑하면 안나온다.  
특히 Reducer에서 빌드오류가 발생하면 정확한 이유를 안알려 주는 경우가 많았다.   
예를들어 action이 추가되어서 switch에 case를 추가해야 하는 이슈인데 다른 이유를 말해주는 경우가 많았다 ㅂㄷㅂㄷ   


# 의문
## 1. TCA .run에서 state 값에 접근할 수 없음
왜그럴까? set을 안되게 하는것은 사이드 이펙트가 발생할 수 있기 때문에 이해가 되는데   
get으로도 접근 못하는 이유는 뭘까? 그냥 읽기만 하겠다곰..

## 2. TCA 에서 중복 Array 안보임(id가 중복되는건 안보여짐.)
ScrollView -> VStack 에서 데이터를 보여주는데   
firestore에서 LoadMore 구현 하는데 마지막 데이터를 하나 더 내려줬음   
그래서 Array에 A B C C D E 이런식으로 데이터가 들어갔는데  
C가 하나는 보이고 하나는 빈화면으로 자리만 차지함 ㄷㄷ
오류 수정은 했는데 왜 안보여주지ㅋㅋ   
ForEach에서 id가 같아서 그럴거 같기도 한데 그러면 영역은 왜 잡는거임ㅋㅋ
