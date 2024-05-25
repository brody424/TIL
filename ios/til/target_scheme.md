# Xcode Target, Scheme 알아보기.

## Target

<img width="161" alt="스크린샷 2024-05-25 오전 9 48 20" src="https://github.com/brody424/TIL/assets/15370950/9cd36a0b-5746-4e8e-8712-ada935ef272a">  

타겟은 프로젝트 특정 빌드 구성 요소를 정의하고 관리할 수 있게 도와줍니다.
위의 이미지에서는 test 프로젝트 내에서 각기 다른 애플리케이션, 라이브러리를 생성하는데 사용됩니다.
각 타겟은 독립적이게 구성할 수 있습니다.  


<img width="259" alt="스크린샷 2024-05-25 오전 9 52 34" src="https://github.com/brody424/TIL/assets/15370950/760e984d-049e-4f4c-abd0-197212cfb672">

위의 이미지를 보면 Test Target에는 포함되어 있지만 TestTests는 포함되어 있지 않죠 😀

Product, Staging, Develop 앱을 따로 관리할 수 있는데 이때 사용했던 것이 Target이였어요.  

Target마다 전처리를 사용해서 코드를 분리해줄 수도 있습니다.  

## Scheme
스킴은 어떤 타겟을 어떻게 빌드할 지 설정할 수 있습니다.
주로 빌드, RUN, TEST, 아카이빙 등의 워크플로우를 정의하고 관리할 수 있습니다.  

<img width="935" alt="스크린샷 2024-05-25 오전 10 04 01" src="https://github.com/brody424/TIL/assets/15370950/6c966001-6975-441f-9f24-7291e5a9d1e6">


새로운 스킴을 만들 때 Target, Name을 정의해야 합니다.   
<img width="488" alt="스크린샷 2024-05-25 오전 10 03 34" src="https://github.com/brody424/TIL/assets/15370950/15f6b0a4-0300-4bd2-b4ff-333c6f8ff7f5">

