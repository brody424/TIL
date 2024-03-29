# 면접사전 1차 개발 회고

생각보다 오래걸린 사이드 프로젝트.

기존에 면접에 도움을 주는 앱을 만들자! 라는 컨셉은 지켜져서 다행이다.

역시 개인앱에서 가장 힘든것은 **마음먹기** 이다. 

<br/>

## 컨셉
- 면접공부를 시작할 때 어떤것부터 시작해야 될지 모르기 때문에 면접을 도와주는 앱을 만들어보자!  
- 서버에서 직종을 제공하고 문제를 제공하자!
- 자신만의 면접사전을 만들어서 연습할 수 있게 하자!

<br/>

## 좋았던 점  
- MVVM-C 패턴으로 2개의 프로젝트를 진행해서 조금더 MVVM-C 사용법에 익숙해진 느낌.
- Swinject를 이용한 DI 적용.
    - 결합도 감소, 테스트 용이성, 코드 재사용성 증가라고 하는데 아직 잘 와닿지 않아서 몇번 더 써봐야 할듯.
    - 그래도 Coordinator에서 ViewModel을 관리하고 주입해줘서 ViewController에서 신경을 안써서 좋았다.
    - V:VM 이 1:1이 아닌 1:N으로 사용할 수 있어서 좋았다.(하지만 잘 분리해야 할듯. 면접사전에서는 VieWModel 하나가 너무 커져서 문제가 되었음.)
- 클라이언트 개발자로써 JWT를 가져다 쓸줄만 알았지 어떻게 만들어지고 어떻게 동작하는지 알게되어서 좋았음
- 이전 개인앱인 앱돈은 리젝을 10번 이상 당했는데 이번에는 2번으로 끝내서 좋았다.
    - 앱에 연락방법 제공
    - GPT(써드파티) 이름 빼기 
- letsencrypt라는 무료 ssl을 알게되어서 좋다. 앞으로 개인앱할때는 https 사용하자. (굳이 .app 도메인을 안사도 될거같다.)
- 오랜만에 개인앱 만들었는데 만들고 나니까 뿌듯하고 기분이 좋다.

<br/>

## 아쉬웠던 점
- 프로토타입 앱을 오래동안 사용했기 때문에 최초 기획, 설계에서 많이 안바뀔줄 알았는데   
디자인을 하거나... 서버를 개발하거나... 클라이언트를 개발하거나 할때 많이 바뀐게 많아서 아쉬웠다.  
- 후반으로 갈 수록 집중력이 떨어져서 아직 더 작업할게 남아있다. (다국어, 푸시, 문제요청 ...)
- ViewModel 설계를 잘못해서 너무 많은 기능을 처리하며 1:N 관계에 너무 집착하지 말자. 
- 디자인이 아쉽다.. 
    - 최대한 심플하게 디자인을 하려고 했지만 흠.. 그래도 아쉽다
    - 그래도 앱 아이콘은 잘 뽑은듯!? 

## Next Step
- 면접사전 고도화 ㄱㄱ
    - 다국어
    - 설정화면
    - FCM
    - Firebase RemoteConfigure
        - GPT 프롬프트 고도화
        - 선택업데이트, 강제업데이트 개발
    - 데이터 작업(질문, 직종 추가)

- 다음 개인앱 ㄱㄱ
    - 플러터 찍먹 가보자.
    - 식단관련 앱 만들어보자.!

## 총평
오랜만에 개인앱을 만들었는데 프로토타입으로 사용하고 있는 앱들을 정식버전으로 개발해보자.  
오픈하니까 사용하는 사람은 많이없어도 뿌듯하고 좋네. 