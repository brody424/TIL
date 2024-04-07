# DeepLink, URI SCheme, Universal Link, Dynamic Link

## 딥링크
링크를 눌렀을 때 앱이 실행되거나 앱 내 특정 화면으로 이동시키는 기능

딥링크는 크게 3가지 방식이 있다.
- URI Scheme 
- App Link(Android에서 사용)
- Universal Link(iOS에서 사용)

그리고 AppLink + Universal Link인 Firebase 에서 제공하는 Dynamic Link 


## URI Scheme
앱에 URI 스킴 값을 등록하여 딥 링크를 사용할 수 있다.  

- Scheme://Path 

형식으로 사용한다. 

아래처럼 사용하는 애들이다. 

```
yotubue://
twitter://
myapp:// 
```

iOS 에서는 URL Scheme 항목에 스킴 값을 입력하여 사용한다.

<img width="668" alt="스크린샷 2024-04-07 오후 9 06 03" src="https://github.com/brody424/TIL/assets/15370950/9ecf6d2d-acea-492b-ada8-9f341b1fa4c9">

위에처럼 URL Schemes 를 등록하여 사용할 수 있다.

단점으로는 중복이 될 수 있다.  
그리고 중복되면 내가 원하지 않는 다른 앱으로 이동시켜버릴 수 있다.

그래서 나온것이 iOS에서는 유니버셜링크, Android에서는 앱링크이다.

## Universal Link

고유한 도메인 주소를 딥링크 실행값으로 사용할 수 있다.  
앱 번들 ID를 미리 특정 도메인의 웹사이트에 등록하여 소요권을 증명해야 한다.  

<img width="720" alt="스크린샷 2024-04-07 오후 9 15 26" src="https://github.com/brody424/TIL/assets/15370950/4d00d81f-b33a-4df0-b19a-127b7dcf57a2">


유니버셜 링크를 클릭했을 때 
- 앱이 설치되어 있는 경우
    - 앱 열기 혹은 앱의 특정 페이지로 열기
- 앱이 설치되어 있지 않는 경우
    - 해당 도메인의 웹페이지로 이동


Deferred Deep Link 란것도 있다.
지연된 딥링크로 앱이 설치되어 있지 않을 때 앱 설치 후 특정 페이지로 랜딩이 가능하다.

단점으로는
유니버셜링크는 애플에서 만든 앱 이외에서는 정상적으로 동작하지 않는다.ㅠ


## Dynamic Link
앞에서 안드로이드용 AppLink와 iOS용 Universal Link가 있다고 했는데 안드로이드와 iOS를 하나의 링크로 관리할 수 있게 도와준다.

Firebase에서 지원하는 기능!