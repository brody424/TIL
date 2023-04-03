# [Xcode Cloud의 workflow 구성하는 방법](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow)
### Xcode Cloud를 사용하고 지속족 통합 및 배포를 채택하도록 프로젝트 또는 workspce를 구성하세요.

<br><br>

## [Overview](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Overview)

Xcode는 Xcode Cloud를 사용하도록 프로젝트 또는 workspaceㄹ르 구성하는데 도움을 줍니다.
프로젝트를 분석하고, Xcode Cloud를 사용하여 앱 또는 프레임워크를 신속하게 구축할 수 있는 구성을 제안하고, 첫 번째 빌드를 완료한 후 지속적인 통합 작업을 쉽게 개선할 수 있습니다.  

Xcode Cloud의 정보를 더 얻으려면 [WWDC21: Meet Xcode Cloud](https://developer.apple.com/wwdc21/10267), [WWDC21: Explore Xcode Cloud workflows](https://developer.apple.com/wwdc21/10268), [WWDC21: Customize your advanced Xcode Cloud workflows](https://developer.apple.com/wwdc21/10269), [WWDC22: Get the most out of Xcode Cloud](https://developer.apple.com/videos/play/wwdc2022-110374)을 확인하세요.

<br>

### [Xcode Cloud workflow 검토](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Review-Xcode-Cloud-workflows)

Xcode Cloud를 사용하기 위해 프젝트 또는 workspace 구성을 시작하면 Xcode는 프로젝트 분석하여 해당 설정을 감지하고 _products_ 라고 하는 앱 및 프레임워크를 나열합니다.  
제품을 선택하면 Xcode가 첫번째 작업 흐름(workflow)을 제안합니다.  
_작업 흐름(workflow)_ 는 Xcode Cloud에서 수행할 단계에 대한 구성입니다.

workflow에서는 다음 설정이 포함됩니다:

-   workflow의 이름과 설명같은 일반정보

-   임시 빌드 환경의 Xcode 및 macOS 버전  
Xcode Cloud는 사용 가능한 macOS 및 Xcode 버전을 주기적으로 업데이트할 수 있으며 성공적인 빌드를 계속하기 위해 workflow를 업데이트하도록 요청할 수 있습니다.

- Xcode Cloud가 workflow를 실행하는 시기를 정의하는 시작조건
    
- Xcode Cloud가 수행하는 작업  
workflow에 대해 여러가지 작업을 구성할 수 있습니다. 사용가능한 작업은 빌드,분석,테스트 및 아카이브 입니다.

-  Xcode Cloud가 수행하는 사후조치(빌드 후 작업 선택)  
예를 들어 사용자 지정 알림을 구성하거나 [TestFlight](https://developer.apple.com/testflight/)를 통해 테스터에게 앱의 새 버전을 배포할 수 있습니다.

- 사용자 지정 작업을 수행하는 _Custom build scripts_   
예를 들어 third-party tool이 있습니다. 자세한 내용은 [Writing custom build scripts](https://developer.apple.com/documentation/xcode/writing-custom-build-scripts) 에서 확인하세요.
    
제안된 workflow를 검토한 후 Xcode Cloud와 Git 저장소를 연결하고 _build_ 라고 하는 workflow를 실행합니다.

빌드가 완료되면 Xcode Cloud는 다음을 수행합니다:

-   XCode의 _build reports_ 링크 및 [App Store Connect](https://appstoreconnect.apple.com/)를 포함하여 빌드에 대한 정보가 포함된 이메일을 보냅니다.

- 빌드의 _artifacts_ 를 저장하고 Xcode 또는 App Store Connect에서 다운로드할 수 있도록 합니다. 

XCode Cloud가 생성하는 아티팩트(artifacts)는 아래와 같습니다.

-   Detailed build logs
    
-   내보낸 앱 아카이브, app binary 또는 프레임워크

-   자동화된 UI 테스트에서 생성한 스크린샷을 포함한 테스트 결과 번들 


Xcode를 사용하여 초기에 Xcode Cloud를 사용하도록 프로젝트 또는 workspace를 구성하세요.  
첫번째 빌드를 시작한 후 Xcode 또는 App Store Connect를 사용하여 추가 workflow를 구성하고 빌드 정보에 액세스하는 등의 작업을 수행할 수있습니다.
자세한 내용은 [Edit and create workflows in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Edit-and-create-workflows-in-App-Store-Connect). 를 참고하세요.


### [아카이브 작업 선택](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Select-the-archive-action)

Xcode Cloud를 사용하여 빌드하려면 각 앱 또는 프레임워크에 대해 해당 체계가 아카이브 작업을 사용하는지 확인하세요.

![아카이브 작업이 활성화된 Fruta 앱의 스킴을 보여주는 스크린샷](https://docs-assets.developer.apple.com/published/e945f8514d710390e42820ba221438d6/Configuring-Your-First-Xcode-Cloud-Workflow-1@2x.png)  
아카이브 작업이 활성화된 Fruta 앱의 스킴을 보여주는 스크린샷

아카이브 작업을 사용하는 프로젝트의 스킴을 찾으려면 터미널에서 다음 명령을 실행하세요.
```
xcodebuild -project Example.xcodeproj -describeAllArchivableProducts -json
```

Xcode는 동일한 명령을 사용하여 Xcode Cloud로 빌드할 수 있는 사용 가능한 product을 찾습니다.

### [product을 선택하세요.](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Choose-a-product)

Xcode Cloud를 사용하도록 프로젝트 또는 workspace을 구성하려면 Xcode에서 프로젝트 또는 workspace를 실행하세요.  
Report navigator가 표시되는지 확인하고 클라우드 탭을 선택한 다음 workflow 만들기를 선택하세요. 

![Xcode Cloud를 사용하지 않는 프로젝트의 Report navigator와 Cloud 탭을 보여주는 Xcode 스크린샷](https://docs-assets.developer.apple.com/published/bf5e5a2b8f28d0e81c0e83addac97313/Configuring-Your-First-Xcode-Cloud-Workflow-2@2x.png)  
Xcode Cloud를 사용하지 않는 프로젝트의 Report navigator와 Cloud 탭을 보여주는 Xcode 스크린샷  

Xocde는 프로젝트 또는 workspace를 분석하고 product 선택 시트에서 찾은 각 product를 나열합니다.  
앱 또는 프레임워크와 일치하는 product를 선택하고 다음을 클릭하세요.

![Fluta 앱의 모든 제품을 나열하는 Xcode product 선택 시트의 스크린샷](https://docs-assets.developer.apple.com/published/07ee5eb73ec5e84c53b4716d0a544368/Configuring-Your-First-Xcode-Cloud-Workflow-3@2x.png)  
Fluta 앱의 모든 제품을 나열하는 Xcode product 선택 시트의 스크린샷

프로젝트에 동일한 번들 식별자를 사용하는 대상이 포함된 경우 Xcode Cloud는 이를 하나의 product로 간주합니다.  
product에는 하나의 번들 ID만 가질 수 있으며 번들 ID는 항상 정확히 하나의 Xcode Cloud product과 일치합니다.  
프로젝트 또는 workspcae에 여러 앱 대상이 포함된 경우 :

-   가능한 경우 각 플랫폼에 대해 앱 버전 간에 동일한 번들 ID를 공유하세요.  
예를들어 앱의 iOS, macOS 및 watchOS버전에 동일한 번들 ID를 사용합니다.  

-   앱의 각 버전이 다른 번들 ID를 사용하는 경우(예: iOS 버전이 `com.example.myiosapp`를 사용하고 macOS 앱이 `com.example.mymacapp`을 사용하는 경우) Xcode는 둘 이상의 product을 감지합니다. 첫 번째 workflow를 생성할 때 그 중 하나를 선택하고 나중에 다른 product에 대한 추가 workflow를 구성해야 합니다.
    
당신은 둘 이상의 Apple Developer 팀의 구성원일 수 있습니다.  
이 경우 Xcode는 팀을 선택하도록 요청합니다.  
TestFlight로 테스터에게 배포하고 App Store에 앱을 게시하는 데 사용할 팀을 선택할 수 있습니다.


### [제안된 workflow 검토하기](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Review-the-suggested-workflow)


선택한 product를 기준으로 Xcod는 다음과 같은 첫번째 workflow를 제안합니다 :  

-   Git 저장소의 기본 branch에 대한 각 변경 사항과 기본 브랜치를 대상으로 하는 PR마다 빌드를 시작합니다.

-   임시 빌드 환경을 위해 최신 출시된 macOS 및 Xcode 버전을 사용합니다.

-   아카이브 Action을 사용합니다.

-   Xcode 및 App Store Connect의 빌드 보고서에 대한 링크를 포함하여 빌드에 대한 정보가 포함된 이메일을 보냅니다.  

<br>
<br>

첫번째 빌드를 시작하기 전에 제안된 workflow를 검토하세요. 예를 들어 Xcode가 올바른 scheme을 선택했는지 확인하세요.

1.  Review workflow 시트에서 Edit workflow를 클릭합니다.

2. workflow 정보를 표시하는 시트에 필요한 경우에만 변경하고 저장됩니다.![A screenshot of the suggested workflow in Xcode for the Fruta app.](https://docs-assets.developer.apple.com/published/f00fe5c3b5dd12f29978feca81c98da4/Configuring-Your-First-Xcode-Cloud-Workflow-4@2x.png)  Fruta 앱에 대한 Xcode 에서 제안된 workflow 스크린샷
    
3.  Review workflow 시트에서 Next를 클릭하고 Xcode가 소스 코드에 대한 Xcode Cloud 액세스 권한을 부여하는 프로세스를 안내하도록 합니다. 
    

### [소스코드에 대한 Xcode Cloud 액세스 권한 부여](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Grant-Xcode-Cloud-access-to-your-source-code)

Xcode Cloud는 코드가 포함된 Git 저장소에 대한 엑세스 권한이 필요합니다.  
이 액세스를 사용하여 코드가 변경할 때 자동으로 코드를 빌드하고 테스트합니다.  
Xcode Cloud를 사용하도록 프로젝트 또는 workspace를 구성하면 Xcode는 이를 분석하여 사용하는 SCM(Source Code Management) 공급자를 감지합니다.  
소스 코드에 대한 액세스 권한 부여 시트에서 액세스 권한 부여를 클릭하고 Xcode가 SCM 공급자의 기본 인증 프로세스를 안내하도록 합니다.    

![A screenshot of the Grant Access to Your Source Code sheet in Xcode.](https://docs-assets.developer.apple.com/published/370da2c2fcf558030fd8b44500dac3b6/Configuring-Your-First-Xcode-Cloud-Workflow-5@2x.png)  Xcode의 소스 코드 시트에 대한 액세스 권한 부여 스크린샷 

Xcode Cloud가 Git 저장소에 액세스하도록 허용한 경우 Xcode는 소스코드에 액세스 할 수 있음을 나타냅니다. (다음을 클릭하세요)

소스 코드에 대한 Xcode Cloud 액세스 권한 부여에 대한 추가적인 지침은 [Source code management setup](https://developer.apple.com/documentation/xcode/source-code-management-setup)을 확인하세요.

### [Create an app record](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Create-an-app-record)

Xcode Cloud는 Xcode, [TestFlight](https://developer.apple.com/testflight/), 그리고 App Store Connect를 강력한 CI/CD 시스템으로 결합합니다.  
결과적으로 앱에 대한 App Store Connect의 앱 레코드가 필요합니다. (App Store Connect의 앱생성을 하세요.)

만약 이미 앱에 대해 App Store Connect에서 앱 레코드를 생성한 경우 Xcode Cloud에서 자동으로 사용합니다.  
앱에 대한 앱 레코드를 만들지 않은 경우 Xcode는 Git 저장소에 대한 Xcode Cloud 액세스 권한을 부여한 후 생성할 수 있도록 도와줍니다.

앱 레코드를 생성하려면 Apple Developer Team의 앱 관리자, 관리자 또는 계정 소유자의 역할이 있어야 합니다.  
개발자 역할이 있는 경우 앱 만들기 권한이 필요합니다.  
필요한 역할 또는 권한이 없는 경우 [Create an app record in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Create-an-app-record-in-App-Store-Connect)를 참고하세요.


### [첫번째 빌드 시작하기.](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Start-your-first-build)

이제 Git 저장소에 대한 Xcode Cloud 액세스 권한을 부여하고 해당하는 경우 앱 레코드를 생성했으므로 첫번째 빌드를 시작할 준비가 되었습니다.  
브랜치를 선택하고 빌드 시작을 클릭하세요.
Xcode Cloud는 브랜치를 확인하고 코드를 빌드하기 시작합니다.

![Fruta 앱용 Xcode의 Start Build 시트 스크린샷](https://docs-assets.developer.apple.com/published/7450d7c337f31cf4f7a578035b6be55c/Configuring-Your-First-Xcode-Cloud-Workflow-6@2x.png)  Fruta 앱용 Xcode의 Start Build 시트 스크린샷


편집기 창에서 진행 중인 빌드에 대한 정보를 보려면 Report navigator에서 빌드를 선택하세요.  
자세한 빌드 로그를 보려면 보고서 개요(Report Outline)에서 작업을 확장하고 로그를 선택하세요.  
보고서 개요(Report Outlines)가 보이지 않으면 편집기 창 오른쪽 상단에 있는 편집긴 옵션 조정 버튼을 사용하여 활성화 하세요.   

Xcode Cloud는 프로젝트 빌드를 완료하면 빌드에 대한 정보(빌드 상태, 빌드에 사용된 커밋, Xcode 또는 App Store Connect의 빌드 보고서 링크)가 포함된 이메일을 보냅니다

### [빌드가 실패한 이유에 대한 이해](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Understand-why-a-build-failed)

첫 번째 빌드가 실패할 가능성이 있습니다.  
특히 복잡한 코드 베이스와 종속성이 있는 프로젝트에서는 실패할 가능성이 높습니다.  
빌드가 실패한 이유를 이해하려면 Report navigator에서 빌드를 선택하고 보고서 개요에서 실패한 작업을 확장한 다음 로그를 선택하여 빌드 로그를 확인하세요.  

![A screenshot of Xcode that shows detailed build information in the Editor pane for a failed archive action.](https://docs-assets.developer.apple.com/published/ba9dafa2ed0f6e806ddcbfc73e585049/Configuring-Your-First-Xcode-Cloud-Workflow-7@2x.png)  
실패한 아카이브 작업에 대한 자세한 빌드 정보를 Editor 창에 표시하는 Xcode의 스크린샷   

아티팩트를 선택하고 빌드 보고서를 다운로드할 수도 있습니다.  
일반적인 빌드 문제 해결에 대한 추가 지침은 [Resolving common configuration and build issues](https://developer.apple.com/documentation/xcode/resolving-common-configuration-and-build-issues)를 확인하세요.

Xcode로 빌드 로그를 보는 것 외에도 App Store Connect에서 빌드 로그를 탐색할 수 있습니다.

App Store Connect에서 빌드 로그를 보려면 다음과 같이 하세요.

1. App Store Connect에 로그인 하고 앱 페이지로 이동합니다.

2. Xcode Cloud 탭을 선택하세요.

3. 사이드바에서 빌드를 선택하세요.

4. workflow를 확장하고 빌드를 선택하세요.

5. 사이드바에서 작업을 확장하고 로그를 선택하세요.
    

### [지속적 통합 연습 다듬기](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Refine-your-continuous-integration-practice)

첫 번쨰 workflow를 구성하고 첫 번째 빌드를 성공적으로 완료한 후 다음 단계를 계획하는데 시간을 할애하여 CI/CD 프로세스를 개선합니다. 예를 들어 

-   [Connect your personal SCM account to Xcode Cloud](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Connect-your-personal-SCM-account-to-Xcode-Cloud)에 설명된 대로 동료에게 Xcode Cloud 사용을 시작하도록 요청합니다.

-   첫번쨰 workflow의 이름과 설명을 변경하세요.    

-   unit test를 실행하는 첫 번째 workflow에 테스트 action을 추가하세요.    

-   사용자 지정 branch를 업데이트하거나 시작 조건을 추가하는 경우에만 빌드를 시작하도록 첫 번째 workflow의 시작 조건을 변경합니다. 

-   [TestFlight](https://developer.apple.com/testflight/)를 사용하여 테스터에게 앱의 새 버전을 배포하는 사후작업을 추가합니다.


-   완료하는데 오래걸리는 고급 검증을 수행하기 위해 workflow를 만드세요. 예를들어 자동화된 UI 테스트를 일주일에 한번 실행하는 workflow를 만드세요.

-   첫 번쨰 workflow를 만들 때 Xcode가 감지한 다른 product에 대한 workflow를 만드세요.
    
- 인기 협업 도구인 [Slack](https://slackhq.com/)에서 빌드 정보를 받습니다. Xcode Cloud를 Slack에 연결하는 방법에 대한  자세한 내용은  [Connecting Xcode Cloud to Slack](https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-slack)를 참고하세요.
    
- PR을 병합하기 전에 가능한지 확인하려면 Xcode Cloud 빌드가 필요합니다. 자세한 내용은 [Configuring requirements for merging a pull request](https://developer.apple.com/documentation/xcode/configuring-requirements-for-merging-a-pull-request)를 확인하세요.
    

### [Xcode에서 추가 workflow 만들기](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Create-additional-workflows-in-Xcode)

Xcode 에서 추가 workflow를 구성 및 생성하거나 기존 workflow를 변경하려면 다음을 수행하세요.

1. Report Navigator에서 Cloud tab으로 이동하세요.

2. 앱 이름 workflow를 컨트롤 + 클릭하고 workflow 관리를 선택하세요.

3. workflow 관리 시트에서 workflow를 두번 클릭하여 변경하거나 추가버튼(+)를 사용하여 새 workflow를 추가할 수 있습니다.
    
custom workflow를 만드는 자세한 방법이 궁금하다면  [Developing a workflow strategy for Xcode Cloud](https://developer.apple.com/documentation/xcode/developing-a-workflow-strategy-for-xcode-cloud) 와 [Xcode Cloud workflow reference](https://developer.apple.com/documentation/xcode/xcode-cloud-workflow-reference)를 확인하세요.

### [App Store Connect에서 workflow를 수정하거나 만들기](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Edit-and-create-workflows-in-App-Store-Connect)

Xcode에서 첫번쨰 Xcode Cloud workflow를 구성해야 하며, Xcode Cloud와 Xcode의 심층적으로 통합하면 코드를 작성하고, 변경사항을 검토하고, 빌드 정보를 확인하고, workflow를 구성하는 통합 개발 프로세스가 가능합니다.  
그러나 팀에는 Xcode에 익숙하지 않은 전담 인프라 엔지니어 또는 릴리스 관리자가 있을 수 있습니다.  
이를 수용하고 workflow를 구성하고 빌드 정보를 볼 수 있는 추가 방법을 제공하기 위해서 App Store Connect는 Xcode Cloud와도 긴밀하게 통합됩니다.

App Store Connect에서 workflow를 보거나 편집하거나 생성하려면 아래와 같이 하세요.

1. App Store Connect에 로그인하고 앱 페이지로 이동하세요.

2. Xcode Cloud 탭을 클릭하세요.

3. 사이드바에서 workflow 관리를 선택하세요.

4. workflow 관리 옆에 있는 추가 버튼을 클릭하여 새 workflow를 만들거나 workflow를 클릭하여 해당 설정을 보고 편집하세요.

workflow의 자세기 버튼(...)을 클릭하면 workflow의 편집, 복제, 비활성화, 삭제를 할 수 있습니다.

### [빌드 아티팩트 다운로드 및 보관](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Download-and-archive-build-artifacts)

Xcode Cloud가 workflow 빌드를 완료하면 빌드 정보, 앱 바이너리, symbol 정보, 테스트 결과 등을 포함하는 아티팩트 집합을 생성합니다. Xcode Cloud는 빌드를 완료한 후 최대 30일 동안 아티팩트를 저장합니다.

과거 빌드를 보관해야 할 필요성 외에도 App Store 에서 배포하는 앱 버전의 빌드 아티팩트를 다운로드하고 보관하는 것이 특히 중요합니다. Xcode Cloud가 크래시 리포트를 사용하여 문제를 진단하기 위해 앱을 보관할 때 Xcode Cloud에서 생성하는 기호 정보가 필요할 수 있기때문입니다.

빌드 정보 및 아티팩트를 다운로드 하려면 Xcode 또는 App Store Connect를 사용하세요. 또는 App Store Connect API를 사용하여 빌드 아티팩트 다운로드 작업을 자동화하세요. 

App Store Connect API로 Xcode Cloud를 자동화하는 방법에 대한 자세한 내용은  [Xcode Cloud Workflows and Builds](https://developer.apple.com/documentation/appstoreconnectapi/xcode_cloud_workflows_and_builds)를 확인하세요. 

symbol 정보나 크래시 리포트에 대한 정보는  [Diagnosing issues using crash reports and device logs](https://developer.apple.com/documentation/xcode/diagnosing-issues-using-crash-reports-and-device-logs)를 참고하세요.


<br><br><br>

### 비고
2762 words  
234 lines  
소요시간 2:15:30