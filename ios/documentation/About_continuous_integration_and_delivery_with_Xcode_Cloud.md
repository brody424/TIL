# Xcode Cloud를 통한 지속적 통합 및 배포 정보
### Xcode Cloud를 사용한 지속적 통합 및 배포가 어떻게 고품질의 앱과 프레임워크를 만드는데 도움이 되는지 배울수 있습니다.


<br>
<br>

## [Overview](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Overview)
---
Xocde는 Apple 플랫폼의 앱 또는 프레임워크를 빌드, 테스트, 릴리즈 하는데 사용하는 도구모음으로 구성됩니다.  
기능을 추가하고 더 많은 디바이스와 플랫폼을 지원하면 앱 또는 프레임워크와 코드베이스의 복잡성이 증가하여 품질을 보장하기가 더 어려워 집니다. 
Xcode Cloud를 사용하면 앱과 프레임워크의 품질을 모니터링, 보장, 개선하는 표준방식인 지속적인 통합 및 배포(CI/CD)를 채택할 수 있습니다. 

<br>

Xcode Cloud는 소스 관리를 위해 Git을 사용하는 CI/CD 시스템으로 코드베이스의 품질과 안정성을 보장하는 통합 시스템을 제공합니다.  
또한 앱을 효율적으로 개시(publish) 하는데 도움을 줍니다.  
Xcode Cloud는 [Xcode](https://developer.apple.com/xcode/)와 Apple 클라우드 인프라와 결합하여 코드를 빌드하고, 테스트하며 [TestFlight](https://developer.apple.com/testflight/) 와 [App Store Connect](https://appstoreconnect.apple.com/)를 함께 사용함으로써 앱을 쉽게 배포할 수 있게 해줍니다. 
-   코드를 자동으로 빌드하고 테스트 합니다.

-   시뮬레이터에서 Apple 기기의 앱을 자동으로 자주 테스트 합니다.

-   Xcode Cloud에서 알림을 받아 심각한 문제가 되기 전에 오류를 식별합니다.

-   TestFlight를 사용하여 팀 구성원이나 테스터에게 새로운 버전의 앱을 배포합니다.

-   App Store에 게시하기 전에 새 버전의 앱을 검토에 사용할수 있도록 앱을 빌드합니다.(?)

-   Xcode와 Apple 클라우드 인프라를 사용하여 공동으로 소프트웨어를 개발합니다. (여러 개발자가 Xcode Cloud를 통해 동시 작업이 가능하며, Apple 클라우드 인프라를 활용하여 소프트웨어를 개발하고 관리합니다.)
    

![문제를 해결하고 변경 사항을 확인하기 위해 피드백을 구축,테스트,배포 및 수집하는 반복적인 통합 및 배포 프로세스를 보여주는 그림입니다.](https://docs-assets.developer.apple.com/published/bca93b3fc3895d146eeb3773171a9c1f/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-1@2x.png)
문제를 해결하고 변경 사항을 확인하기 위해 피드백을 구축,테스트,배포 및 수집하는 반복적인 통합 및 배포 프로세스를 보여주는 그림입니다.

<br>

CI/CD의 모든 것을 한번에 도입할 필요는 없습니다.   
대신 느리지만 꾸준한 접근 방식을 취하고 [Git](https://git-scm.com/) 을 사용하여 프로젝트 소스 코드를 관리하세요.  
Git의 워크플로우와 프로세스에 익숙해진 후에는 가장 기본적인 수준에서 Xcode Cloud를 사용하세요. (빌드, 테스트 구성)  
Xcode Cloud의 작동 방식에 익숙해지면 TestFlight로 테스트 빌드를 제공하는 것과 같은 CD 기능을 활용할 수 있습니다.

<br><br>
### [The role of source control](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#The-role-of-source-control)

앱 코드의 변경을 관리하는것은 어려울 수 있습니다.  
특히 동시에 여러 변경 작업을 수행하는 경우에는 더욱 어렵습니다.  
소스코드 변경 사항을 구성,관리 및 문서화 하는데 도움이 되도록 Xcode에서는 Git을 사용한 소스 제어를 지원합니다.  
소스 제어를 사용하면 코드 변경 사항을 추적하고 검토하여 프로젝트 품질을 향상시킬 수 있습니다.

<br>

소스 제어는 CI/CD에서 매우 기본적인 역할을 하기 때문에 Xcode Cloud에서는 코드가 Git 저장소에 있어야 합니다.

Xcode Cloud는 다음과 같은 소스 코드 관리(SCM) 제공 업체를 지원합니다.

-   [Bitbucket Cloud](https://bitbucket.org/) 와 [Bitbucket Server](https://bitbucket.org/product/enterprise)
    
-   [GitHub](https://github.com/) 와 [GitHub Enterprise](https://github.com/enterprise)
    
-   [GitLab](https://gitlab.com/) 와 [self-managed GitLab](https://about.gitlab.com/install)
    

Xcode를 사용한 소스제어에 대한 내용을 자세히 알아보려면 [Source control management](https://developer.apple.com/documentation/xcode/source-control-management)를 참고하세요. 


### [자동화된 빌드 빛 테스트의 중요성](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#The-importance-of-automated-building-and-testing)

일반적으로 개발 프로세스는 코드를 변경하고, 프로젝트를 빌드하고, 시뮬레이터 또는 테스트 디바이스에서 앱을 실행하는 것으로 시작됩니다.  
또한 [XCTest](https://developer.apple.com/documentation/xctest)를 사용하여 작성한 단위 테스트와 통합, 성능 또는 UI Test를 실행하여 변경사항을 로컬에서 검증할 수도있습니다. 

<br>

Xcode는 이러한 작업을 수행하는 데 걸리는 시간을 줄일 수 있지만 여전히 상당한 시간이 걸립니다.  
이는 특히 다양한 장치와 운영 체제를 지원하는 복잡한 앱이나 프레임 워크에 해당됩니다.  
그러나 Xcode Cloud를 사용하면 기존보다 짧은 시간에 여러 시뮬레이션 장치에서 프로젝트를 빌드, 실행 및 테스트할 수 있습니다.  



![자동화된 빌드 및 테스트의 다양한 단계를 보여주는 그림입니다. 변경사항은 자동화된 빌드, 테스트 및 변경사항을 확인하는 기타 작업으로 이어집니다.](https://docs-assets.developer.apple.com/published/2427429f22f500f2ccc63735bf2e77bc/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-2@2x.png)  
자동화된 빌드 및 테스트의 다양한 단계를 보여주는 그림입니다. 변경사항은 자동화된 빌드, 테스트 및 변경사항을 확인하는 기타 작업으로 이어집니다.
<br>

변경 사항을 확인한 후 Xcode Cloud는 이메일을 통해 결과를 자동으로 알려줍니다.  
결과는 Xcode 또는 App Store Connect에서도 확인 할 수 있습니다.  
이를 통해 문제가 되기 전에 오류를 탐지하고 코드의 안정성과 품질에 대해 필요한 확신을 가질 수 있습니다. 

Xcode Cloud를 사용하여 프로젝트를 자동으로 구축하는 방법은 [Configuring your first Xcode Cloud workflow](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow) 문서에서 확인할 수 있습니다..

<br><br>

### [지속적인 배포(CD)](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Continuous-delivery)

지속적 통합(CI)의 반대 측면은 지속적배 배포(CD) 입니다.  
고품질의 앱과 프레임워크를 개발하는 데는 자동 빌드 및 테스트가 중요하지만 팀 구성원 및 테스터로부터 받는 실제 테스트를 대체하지는 않습니다.  
Xcode Cloud가 코드의 변경을 확인하여(CI) [TestFlight](https://developer.apple.com/testflight/) 를 통해 내부 테스터에게 앱의 새 버전을 자동으로 제공(CD)할 수 있습니다.   
Xcode Cloud는 또한 TestFlight를 통해 외부 테스트를 위해 앱에 서명하고 앱 리뷰에 제출할 수 있습니다.
export된 앱 아카이브 또는 프레임워크를 자신의 서버에 업로드할 수도 있습니다.  

![CI통과 후 CD의 다양한 단계를 나타내는 그림입니다. (내부 테스터 배포, 외부 테스터 배포, App Store 배포)](https://docs-assets.developer.apple.com/published/35478ba6fa547f5adb9387b9e78b99b6/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-3@2x.png)  
CI통과 후 CD의 다양한 단계를 나타내는 그림입니다. (내부 테스터 배포, 외부 테스터 배포, App Store 배포

<br><br>

### [Xcode Cloud를 사용한 협업 소프트웨어 개발](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Collaborative-software-development-with-Xcode-Cloud)

Git을 사용한 소스제어는 코드 변경을 관리하는데 도움이 됩니다.  
예를 들어 Git branch를 사용하면 검증되고 안정적인 코드베이스에 영향을 주지 않고 변경할 수 있습니다. 또한 branch는 앱이나 프레임 워크를 팀으로 개발할 때 유용합니다.  
그러나 변경사항을 병합(머지)하고, 충돌 해결 및 코드 변경 확인에는 시간이 많이 걸릴 수 있습니다.  

<br>

코드 리뷰 및 병햡을 쉽게하기 위해서 SCM 공급업체들은 _Merge Request_라고도 불리는 Pull Request(PR)에 대한 기능을 제공합니다.  
PR을 생성할 때 변경 사항을 검토할 준비가 되었음을 팀에 알리고, 팀과 함께 서로의 코드 변경사항을 살펴보고 branch를 병합하기 전에 피드백을 주고받을 수 있습니다.

![Git을 사용한 협업 소프트웨어 개발 워크 플로우를 보여주는 그림입니다.(팀원이 PR을 만들고, 피드백을 수집하고, 피드백을 처리하고 결국 branch를 병합)](https://docs-assets.developer.apple.com/published/352978554e67a633b443c9a585695f1f/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-4@2x.png)  
Git을 사용한 협업 소프트웨어 개발 워크 플로우를 보여주는 그림입니다.(팀원이 PR을 만들고, 피드백을 수집하고, 피드백을 처리하고 결국 branch를 병합)  

팀으로 코드를 개발하거나 동시에 여러 변경 작업을 수행하는 경우 일반적으로는 각 변경에 대해 별도의 branch를 사용한 다음 PR을 만들어 동료로 부터 코드리뷰를 받는것입니다.  
이렇게 하면 오류가 심각한 문제가 되기전에 오류를 식별하는데 도움됩니다.

Xcode를 사용하여 [Bitbucket Server](https://bitbucket.org/product/enterprise), [GitHub](https://github.com/), 또는 [GitHub Enterprise](https://github.com/enterprise)로 Git 저장소를 호스팅하면 PR에 대한 생성,확인 및 코멘트를 수행하고 변경  사항을 코드베이스에 병합할 수 있습니다.

![Xcode Cloud가 PR과 통합되는 방식을 보여주는 그림입니다.(누간가 PR을 생성하면 Xcode Cloud는 이 변경 사항을 감지하고 프로젝트를 빌드하고 구성된 테스트를 실행하고 상태를 PR을 게시합니다.)](https://docs-assets.developer.apple.com/published/b158559eb1ebf931b65b806796cc8445/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-5@2x.png)  
Xcode Cloud가 PR과 통합되는 방식을 보여주는 그림입니다.(누간가 PR을 생성하면 Xcode Cloud는 이 변경 사항을 감지하고 프로젝트를 빌드하고 구성된 테스트를 실행하고 상태를 PR을 게시합니다.

<br>

또한 Xcode Cloud를 구성하여 새로운 PR 또는 기존 PR의 변경 사항을 감지할 수 있습니다.  
변경 사항을 감지하면 Xcode Cloud는 임시 빌드 환경에서 관련 branch를 병합하고 자동으로 프로젝트를 빌드하고 테스트를 실행하여 병합된 코드를 확인합니다.  
변경 사항을 확인한 후 Xcode Cloud는 PR에서 상태 메시지를 추가하여 결과를 알려줍니다.