# CI/CD 구축해보기(1) - Xcode Cloud vs Fastlane 어떤걸 선택할까
 
구축되어 있는 CI/CD를 사용,수정해본적은 있지만 처음부터 구축해보는건 처음이기 때문에 TIL 시작

Xcode Cloud vs fastlane 둘중 어느걸로 선택할까? 

ChatGPT에게 각각의 장단점을 물어봤다.

## Xcode Cloud 장단점

### 장점
- 쉽게 배우고 쉽게 사용이 가능하다.
- 클라우드 기반이여서 Apple 서버에서 앱을 빌드, 테스트 하므로 내 PC에서 느려지는 현상을 방지해준다.
- 여러장치에서 병렬로 테스트가 가능하므로 빠른 테스트와 쉬운 디버깅이 가능하다.
### 단점
- 제한된 사용자 정의 옵션 세트를 제공하므로 다른 CI/CD보다는 유연성이 떨어진다.
- iOS, macOS에서만 사용가능하다. 
- Apple 개발자 계정이 필요하다.
- 유료서비스이기 때문에 소규모 개발팀이나 프로젝트에는 가격이 효율적이지 않다.


## Fastlane 장단점

### 장점
- 플랫폼에 구애받지 않는다. (iOS,Android,RN,Flutter등 다양한곳에서 사용가능)
- 오픈소스 프로젝트이기 때문에 사용자 정의 옵션이 가능하다
- Slack, Jira, Github같은 다양한 타사 도구와 통합되거 개발 프로세스를 쉽게 관리하고 팀원과 협업할 수 있다.
### 단점
- 러닝커브가 크다. Ruby를 사용하기 때문에 Xcode cloud보다 배우기 어렵다.
- 구성 파일이 복작하고 디버그하기 어려울 수 있다.
- 오픈소스 프로젝트이기 때문에 Xcode cloud같은 상용 서비스보다 유지관리 및 업데이트가 더 필요할 수 있다.


## 한마디 요약
Xcode cloud 사용이 쉽지만 기능이 제한이 있다

Fastlane 배우기 어렵지만 여러가지 기능을 사용할 수 있다.


## 무엇을 선택할까?
이번 CI/CD는 회사프로젝트에 넣을 예정이다.

넣으려는 이유는 Xcode 14에서 발생하는 이슈때문이며

현재는 회사 맥북을 다운그레이드하여 Xcode 13에서 빌드하는 식으로 하고있다.

그래서 작업은 아이맥으로 하면서 테플 업데이트는 맥북으로 하는게 매우 불편하기 떄문에 

일단은 빠르게 Xcode Cloud로 구축해보자.

