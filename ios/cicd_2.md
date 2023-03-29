



# CI/CD 구축해보기(2) - Xcode Cloud 시작

러닝커브가 낮고 지금의 불편함(맥북으로 빌드하는)을 해소하기 위해서 빠르게 Xcode Cloud 시작!!

오랜만에 돌아온 Apple documentation의 [Xcode Cloud 문서](https://developer.apple.com/documentation/xcode/xcode-cloud) 번역해서 읽어보기  시작!!


Workflow를 만들어보자

General 탭에서는 이름과 설명, 깃 레파지토리, 프로젝트의 workspace를 선택 가능하다.


Environment에서는 Xcode 버전과 macOS 버전을 선택 가능한데 예전버전을 선택할 수 도있다.

나는 Xcode 13버전에서 빌드를 해야하기 떄문에 이 기능이 마음에 들어서 Xcode cloud를 선택했다.

Branch Changes에는 브랜치의 특정 행동을 트래킹하고 있다가 자동 빌드가 가능하다.

나는 Branch에 파일이 변했을때 Action을하게 세팅했다.

Action은 Archive로 설정하여 내가 트래킹하고 있는 브랜치의 코드가 변했을 때 아카이브하기로 했으며

Post-Action으로 빌드가 완료되었을 때 자동으로 테플에 등록하는것을 목표로 했다.


여기까지는 엄청 쉬워서 그냥 후다닥 하고 빌드를 했는데 결과는 빌드가 안된다..

아마 Pod같은 서드파티 라이브러리에서 문제가 나는거 같다..

Pod 처리하는건 다음에 해봐야겠다.




