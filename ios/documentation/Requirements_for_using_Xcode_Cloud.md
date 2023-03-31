# [Xcode Cloud 사용을 위한 요구 사항](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Developer-account-requirements)
### Xcode Cloud를 사용하려면 프로젝트 또는 작업 공간(workspace)을 구성하기 전에 계정, 프로젝트 및 소스 제어 요구 사항을 검토하세요.


<br>
<br>

## [Overview](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Overview)
---  

Xcode는 Xcode Cloud를 사용하도록 프로젝트 또는 작업공간을 구성하는데 도움이 됩니다.  
원할한 프로세스를 위해 Xcode Cloud 사용에 대한 요구 사항을 검토하고 필요에 따라 변경합니다.  
그런 다음 Xcode Cloud를 사용하도록 프로젝트 또는 작업 공간을 구성하고 CI/CD(지속적인 통합 및 배포) 연습을 시작하세요.

### [개발자 계정 요구 사항](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Developer-account-requirements)  

Xcode Cloud를 사용하려면 다음 작업을 수행해야 합니다:  

-   [Apple Developer Program](https://developer.apple.com/programs/)에 등록해야 합니다.
    
-   XCode 설정의 계정에서 Apple ID를 추가해야 합니다.

-   [App Store Connect](https://appstoreconnect.apple.com/) 에 앱에 대한 앱 기록이 있거나 이를 생성하는 데 필요한 역할 또는 권한이 있어야 합니다.

<br>
    
> Note  
앱 레코드를 생성하려면 Apple Developer 팀의 앱 관리자, 관리자 또는 계정 소유자 역할이 있어야 합니다.  
개발자 역할이 있는 경우 앱 만들기 권한이 필요합니다.  
필요한 역할이나 권한이 없는 경우 권한이 있는 팀원과 함께 작업해야 합니다.  
자세한 내용은 [Create an app record in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Create-an-app-record-in-App-Store-Connect)를 참고해주세요.  

App Store Connect의 역할에 대한 자세한 내용은 [Role permissions](https://developer.apple.com/help/app-store-connect/reference/role-permissions) 를 참고해주세요.  
앱 레코드 생성에 대한 자세한 내용은 [Add a new app](https://developer.apple.com/help/app-store-connect/create-an-app-record/add-a-new-app)를 참고해주세요.


<br>

### [프로젝트 및 workspace 요구 사항](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Project-and-workspace-requirements)

Xcode Cloud를 사용하려면 프로젝트와 workspace에서 아래와 같은 요구사항을 충족해야 합니다.

-   일관된 Xcode 프로젝트 또는 workspace를 사용해야 합니다.

-   Shared Schemes를 사용해야 합니다. 자세한 내용은 [Customizing the build schemes for a project](https://developer.apple.com/documentation/xcode/customizing-the-build-schemes-for-a-project) 에 확인하실 수  있습니다.

-   앱 또는 프레임 워크를 빌드하는 schemes에 대해 아카이브 액션이 활성화 되어 있어야 합니다.  
    
-   Xcode의 새로운 빌드 시스템을 사용해야 합니다. 새로운 빌드 시스템에 대한 자세한 내용은 [Build System Release Notes for Xcode 10](https://developer.apple.com/documentation/Xcode-Release-Notes/build-system-release-notes-for-xcode-10)를 참조하세요. 새 빌드 시스템을 켜는 방법은 [Choose the build system](https://help.apple.com/xcode/mac/#/dev396bc94c7)에서 확인할 수 있습니다.

- 디펜던시 나 써드파티 툴은 Xcode Cloud에서 사용할 수 있습니다. Xcode Cloud에서 툴 및 디펜던시를 사용할 수 있도록 만드는 방법은 [Making dependencies available to Xcode Cloud](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud)에서 확인할 수 있습니다.

- 자동 코드 서명을 사용합니다. 자동 코드 서명에 대한 자세한 내용은  [Signing & Capabilities Workflow](https://help.apple.com/xcode/mac/current/#/dev60b6fbbc7) 와 [Code Signing](https://developer.apple.com/support/code-signing/)에서 참조하세요.

- Xcode 프로젝트 또는 workspace의 Signing & Capabilities 탭에서 앱 대상에 대한 번들 식별자를 설정하세요. '.xcconfig' 파일을 사용하여 번들 식별자를 설정하는 경우 자세한 내용은  [Review Xcode Cloud workflows](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Review-Xcode-Cloud-workflows)에서 확인하세요.
    

### [소스 제어 요구 사항](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Source-control-requirements)


Xcode Cloud를 사용하려면 소스 제어에 Git을 사용해야 합니다.  
XCode에서 Git으로 소스 제어를 사용하는 방법에 대한 자세한 내용은 [Source control management](https://developer.apple.com/documentation/xcode/source-control-management)를 참고하세요.  
XCode Cloud는 다음 _소스 코드 관리_(SCM) 공급자를 지원합니다.

-   [Bitbucket Cloud](https://bitbucket.org/) 와 [Bitbucket Server](https://bitbucket.org/product/enterprise)
    
-   [GitHub](https://github.com/) 와 [GitHub Enterprise](https://github.com/enterprise)
    
-   [GitLab](https://gitlab.com/) 와 [self-managed GitLab instances](https://about.gitlab.com/install)
    

또한 Xcode Cloud를 Git 저장소에 연결하려면 특정 권한 또는 역할이 필요합니다:

-    [Bitbucket Cloud](https://bitbucket.org/) 또는 [Bitbucket Server](https://bitbucket.org/product/enterprise)에서 코드를 호스팅 하는 경우 _administrator_ 권한이 필요합니다.
    
-  [GitHub](https://github.com/) 또는 [GitHub Enterprise](https://github.com/enterprise)에서 Github organization을 사용하지 않고 코드를 호스팅 하는 경우 _organization owner_ 권한 또는 _admin_ 권한이 필요합니다
    
-   [GitLab](https://gitlab.com/), 또는 [self-managed GitLab instance](https://about.gitlab.com/install) 사용하는 경우  _maintainer_ 권한이 필요합니다.

필요한 역할이나 권한이 없는 경우 권한이 있는 팀원과 함께 작업하세요.  
자세한 내용은  [Connect Xcode Cloud to an admin-managed Git repository](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Connect-Xcode-Cloud-to-an-admin-managed-Git-repository).를 확인하세요.