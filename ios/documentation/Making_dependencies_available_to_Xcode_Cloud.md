# Xcode Cloud에서 종속성을 사용할 수  있도록 만들기
### Xcode Cloud를 사용하도록 프로젝트를 구성하기 전에 종속성을 검토하고 Xcode Cloud에서 사용할 수 있도록 합니다.

<br><br>

## [Overview](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Overview)


Xcode Cloud는 Xcode](https://developer.apple.com/xcode/), [TestFlight](https://developer.apple.com/testflight/), 그리고 [App Store Connect](https://appstoreconnect.apple.com/)와 같이 앱과 프레임워크를 만드는 데 사용하는 도구 결합입니다.  
그러나 Xcode 프로젝트 또는 workspace에는 코드를 컴파일하기 위해 추가된 종속성(dependencies) 또는 third-party tools가 필요합니다.  
예를 들어 오픈 소스 커뮤니티에서 만든 라이브러리 또는 Swift Package Dependencies를 사용하여 앱 간에 코드를 재사용하고 공유할 수 있습니다.  


Xcode Cloud가 private dependency 또는 third-party tool에 액세스할 수 없는 경우 프로젝트는 성공적으로 빌드되지 못합니다.  
빌드 실패를 방지하고 Xcode Cloud 사용으르 시작할 때 시간을 절약하려면 프로젝트에서 Xcode Cloud를 사용하도록 구성하기 전에 종속성을 검토하고 종속성에 액세스할 수 있는지 확인하세요.  
  

> Note  
Xcode Cloud가 사용하는 임시 빌드 환경에는 macOS 및 Xcode의 일부인 도구(예: Python)와 타사 종속성 및 도구 설치를 지원하는 추가적인 Homebrew가 포함됩니다.  
자세한 내용은 아래의 "사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치" 섹션을 참고하세요.  


### [Swift 패키지 dependencies 와 git submodules 사용하기.](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-Swift-package-dependencies-and-Git-submodules)


Xcode Cloud는 저장소에 공개적으로 액세스할 수 있는 경우 별도의 구성 없이 Git 하위 모듈을 사용하여 관리하는 Swift Package 및 종속석을 지원합니다.  
private 종속성을 사용하는 경우 Xcode Cloud를 사용하여 종속성에 액세스할 수 있습니다.  
자세한 내용은 아래의 [private dependencie에 대한 Xcode Cloud 액세스 권한 부여](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Grant-Xcode-Cloud-access-to-private-dependencies)를 참고하세요.


CI/CD 환경에서 Swift package 종속성을 사용하는 모범 사례에 따라 Xcode Cloud는 자동패키지 해결을 사용하지 않고 `Package.resolved` 파일을 사용하여 종속성을 해결합니다.  
프로젝트에서 Swift Package 종속성을 사용하는 경우 `Package.resolved` 파일을 Git 저장소에 포함하고 변경 내용을 커밋해야 합니다.  
파일을 `.gitignore` 파일에 포함하지 마세요.  또한 `Package.resolved` 파일이  `$filename.xcodeproj/project.workspace/xcshareddata/swiftpm/Package.resolved`에 있는지 확인하세요.


<br> 

> Note  
예를 들어 사용자 지정 빌드 스크립트에서 설정을 변경하여 Xcode Cloud가 자동 패키지 해결을 사용하도록 강제하면 정의되지 않은 동작이 발생하고 빌드가 실패할 수 있습니다.


지속적인 통합 및 배포 환경에서 Swift Package를 구축하는 방법에 대한 일반적인 정보는  [지속적인 통합 workflow에서 Swift Package 또는 이를 사용하는 앱 구축하기](https://developer.apple.com/documentation/xcode/building-swift-packages-or-apps-that-use-them-in-continuous-integration-workflows)를 확인하세요.

### [private dependencie에 대한 Xcode Cloud 액세스 권한 부여](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Grant-Xcode-Cloud-access-to-private-dependencies)

Xcode Cloud를 사용하도록 프로젝트 또는 workspace를 구성하면 Xcode는 코드를 호스팅하는데 사용하는 소스 코드 관리(SCM) 공급자를 감지합니다.  
또한 사용자 지정 스크립트에서 액세스하는 각 Private Git Submodule, Swift Package 종속석 또는 Git 저장소에 대한 SCM 공급자를 감지하고 이에 대한 Xcode Cloud 액세스 권한을 부여하는데 도움을 줍니다.  
예를들어 [Bitbucket Server](https://bitbucket.org/product/enterprise)로 코드를 호스팅하고 [GitLab](https://gitlab.com/) 에서 private 종속성을 사용하고 있다면 Xcode는 Bitbucket 서버와 GitLab 계정을 모두 Xcode Cloud에 연결합니다.

Xcode Cloud가 액세스할 수 없는 새로운 private pacakage 종속성을 추가하면 다음빌드가 실패됩니다.  
이 문제를 해겷하려면 실패한 빌드의 빌드보고서로 이동하여 Xcode 또는 App Store Connect를 통해 Xcode Cloud를 종속성의 SCM 공급자에 연결해야 합니다.

> Note    
지원되는 SCM 공급자 중 하나를 사용하여 private 종속성을 호스팅해야 합니다.  
지원되는 SCM 공급자에 대한 자세한 내용은 Source code management setup를 참고하세요.

프로젝트를 구축하려면 자체 호스팅 SCM 공급자의 둘 이상의 인스턴스(대규모 팀의 경우 일반적인 경우)에 액세스해야 할 수 있습니다.  
예를 들어 하나의 앱의 코드를 호스팅하고 다른 하나는 종속성을 호스팅하는 두개의 서로 다른 Github Enterprise 인스턴스를 사용할 수 있습니다.  
이  시나리오가 적용되는 경우 Xcode에서 프로젝트에 대한 초기 온보딩 workflow를 완료하고 앱 코드를 호스팅하는 인스턴스를 연결한 다음 첫번째 빌드가 실패하도록 합니다.  
빌드 실패 후 Xcode는 다른 인스턴스를 연결하기 위한 수정을 제안합니다.  

### [Review third-party dependencies](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Review-third-party-dependencies)

[CocoaPods](https://cocoapods.org/) 이나 [Carthage](https://github.com/Carthage/Carthage) 같은 third-party 종속성 매니저 툴을 사용하고 있는 경우나 프로젝트를 성공적으로 구축하려면 추가도구가 필요합니다.   
Xcode Cloud를 사용하기 전에 프로젝트 또는 workspace를 변경해야 합니다.  

third-party tool 및 종속성은 추가 작업이 필요하므로 Xcode Cloud를 사용하도록 프로젝트 또는 workspace를 구성하기 전에 third-party dependencies 를 검토하고 단순화하세요.  
예를 들어 종속성을 Apple에서 제공하는 프레임워크로 대체할 수 있습니다.  
또는 작성자가 종속성을 Swift Pacakage로 제공하는지 확인하세요.  
그렇다면 third-party tool을 사용하도록 Xcode Cloud를 구성하지 않고 패키지를 사용하고 Swift Pacakage Manager에 대한 지원을 활용할 수 있습니다.

Swift Package dependency로 전환하거나 dependency를 제거하는것이 실용적이지 않는 경우 아래 지침에 따라 Xcode Cloud가 종속성 및 필수 도구에 액세스 할 수 있게 하세요.

### [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool)

Xcode Cloud가 빌드를 수행하는데 사용하는 임시 빌드 환경에는 third-party tools or dependencies 이 포함되어 있지 않습니다.  
그러나 임시 빌드 환경에서는 추가 소프트웨어를 설치하는데 사용할 수 있는 [Homebrew](https://brew.sh/)가 포함되어 있습니다.  
예를 들어 Homebrew를 사용하여 [CocoaPods](https://cocoapods.org/) or [Carthage](https://github.com/Carthage/Carthage) 와 같은 종속성 관리자를 설치 할수있습니다.

Homebrew를 사용하여 도구를 설치하는 방법: 

1. Xcode 프로젝트 또는 workspace 옆에 디렉토리를 만들고 이름을 `ci_scripts` 로 지정하세요.  

2. 실행 가능한 셸 스크립트를 만들고 이름을 `ci_post_clone.sh`로 지정한 다음 `ci_scripts` 디렉토리에 저장하세요.  
예를들어 Xcode에서 Shell Script 템플릿을 사용하여 파일을 만든 다음에 터미널에서  `chmod +x ci_post_clone.sh` 을 실행하여 실행 가능한 파일로 권한을 변경하세요.  

3. Xcode에서 사용자 지정 스크립트(`ci_post_clone.sh`)를 열고 Homebrew로 도구를 설치하는데 필요한 명령을 추가하세요.
  

> Note   
사용자 지정 빌드 스크립트를 사용하여 다양한 작업을 수행할 수 있지만 sudo를 사용하여 관리자 권한을 얻을수는 없습니다.  

사용자 지정 스크립트에 대해서 더 자세한 내용이 알고싶다면 [Writing custom build scripts](https://developer.apple.com/documentation/xcode/writing-custom-build-scripts)를 참고하세요.

### [Xcode Cloud에서 CocoaPods 종속성을 사용할 수 있도록 설정하기.](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Make-CocoaPods-dependencies-available-to-Xcode-Cloud)

CocoaPods는 Apple 플랫폼의 오픈 소스 종속성 관리자 중 하나입니다.  
그러나 Xcode Cloud가 빌드를 수행하는데 사용하는 임시 빌드 환경에는 사전 설치된 도구가 제공되지 않습니다.  
CocoaPods를 사용하는 경우 먼저 `Podfile`과 `Podfile.lock` 파일을 모두 커밋했는지 확인하세요. 그런 다음에 다음 옵션중 하나를 결정해야 합니다. 

-   Git 저장소에 `Pods` 디렉토리를 커밋하여 추가합니다.
    
-  `.gitignore` 파일에 추가하여 소스 제어에서 `Pods` 디렉토르를 제외합니다.
    
`Pods` 디렉토리와 그 내용을 커밋하면 Xcode Cloud가 프로젝트 또는 workspace를 빌드할 수 있도록 CocoaPods를 설치할 필요가 없습니다.  
하지만 `Pods` 디렉토리를 추가할 때 소스 코드 저장소가 많은 공간을 차지합니다.  
또한 바이너리 종속성을 커밋하면 Git 저장소의 성능에 영향을 미칠수 있음을 기억하세요.  
Git을 사용할 때 일반적인 문제이며 Xcode Cloud에만 국한되지 않습니다.  

> Tip  
Pods 디렉토리를 커밋하기로 결정했다면 Git LFS 사용을 고려하세요.  
XCode Cloud가 프로젝트를 빌드하는 데 사용하는 임시 빌드 환경에 사전 설치되어 있습니다.

소스 제어에서 `Pods` 디렉토리를 제외하도록 선택한 경우 사용자 지정 빌드 스크립트를 사용하여 CocoaPods를 설치해야 합니다.  
그러나 소스 코드 저장소가 디스크 공간을 덜 차지하고 Git 저장소의 속도를 저하시키지 않는다는 장점이 있습니다.  
사용자 지정 빌드 스크립트를 사용하여 CocoaPods를 설치하려면 아래와 같이 작업하세요:  


1.  [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool)에 설명된 대로 post-clone 스크립트를 생성합니다. (`ci_post_clone.sh`)
    
2. [Homebrew](https://brew.sh/)로 CocoaPods를 설치하고 CocoaPods 종속성을 다운로드 하는 스크립트에 명령을 추가합니다.  
아래의 코드는 이를 위한 기본 스크립트입니다.
    
    ```
    
    #!/bin/sh
    
    
    # Install CocoaPods using Homebrew.
    brew install cocoapods
    
    
    # Install dependencies you manage with CocoaPods.
    pod install
    ```
    

### [Xcode Cloud에서 Carthage dependencies 을 사용하는 방법](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Make-Carthage-dependencies-available-to-Xcode-Cloud)

Carthage는 Apple 플랫폼용 오픈 소스 종속성 관리자 중 하나입니다.  
그러나 Xcode Cloud가 프로젝트를 빌드하는데 사용하는 임시 빌드 환경에는 사전 설치된 도구가 제공되지 않습니다. 
대부분의 프로젝트에서 'carthage copy-frameworks' 명령에 의존하는 프로젝트를 용이하게 하려면 사용자 정의 빌드 스크립트를 사용하여 Carthage를 설치해야 합니다.

1.  [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool)에 설명된 대로 post-clone 스크립트를 생성합니다. (`ci_post_clone.sh`)
    
2.  Homebrew를 사용하여 Carthage를 설치하고 Carthage 종속성을 빌드하기 위해 필요한 명령을 스크립트에 추가하세요. 



### 후기
단어 1347개   
줄 91개  
걸린시간 1시간
조금씩 익숙해져 가는듯...   
몰랐던 단어들은 OneVoca에 저장해놓고 계속 알림받아서 보니까 조금씩 익숙해고 있어서 좋음.