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

Xcode Cloud supports Swift packages and dependencies that you manage using Git submodules without any separate configuration if their repositories are publicly accessible. If you use private dependencies, Xcode Cloud helps you access them. For more information on using them, see [private dependencie에 대한 Xcode Cloud 액세스 권한 부여](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Grant-Xcode-Cloud-access-to-private-dependencies) below.

Following the best practice for using Swift package dependencies in a CI/CD environment, Xcode Cloud doesn’t use automatic package resolution and instead relies on the `Package.resolved` file to resolve your dependencies. If you use Swift package dependencies in your project, make sure to include the `Package.resolved` file in your Git repository and commit any changes to it. Don’t include the file in your `.gitignore` file. Additionally, make sure the the `Package.resolved` file resides at `$filename.xcodeproj/project.workspace/xcshareddata/swiftpm/Package.resolved`.

> Note  
Forcing Xcode Cloud to use automatic package resolution — for example, by changing settings in a custom build script — may result in undefined behavior and failing builds.

For general information on building Swift packages in a continuous integration and delivery environment, see [Building Swift packages or apps that use them in continuous integration workflows](https://developer.apple.com/documentation/xcode/building-swift-packages-or-apps-that-use-them-in-continuous-integration-workflows).

### [private dependencie에 대한 Xcode Cloud 액세스 권한 부여](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Grant-Xcode-Cloud-access-to-private-dependencies)

When you configure your project or workspace to use Xcode Cloud, Xcode detects the source code management (SCM) provider you use to host your code. It also detects the SCM provider for each private Git submodule, Swift package dependency, or Git repository you access in a custom script and helps you grant Xcode Cloud access to it. For example, if you host your code with [Bitbucket Server](https://bitbucket.org/product/enterprise) and use a private dependency you host with [GitLab](https://gitlab.com/), Xcode helps you connect both your Bitbucket Server and your GitLab account to Xcode Cloud.

If you add a new private package dependency that Xcode Cloud can’t access, the next build fails. To resolve the issue, navigate to the failed build’s build report, and let Xcode or App Store Connect help you connect Xcode Cloud to the dependency’s SCM provider.


> Note  
You need to host your private dependencies with one of the supported SCM providers. For more information on supported SCM providers, see Source code management setup.

Building your project may require access to more than one instance of your self-hosted SCM provider — a common case for large teams. For example, you may use two different GitHub Enterprise instances where one hosts your app’s code and the other hosts your dependencies. If this scenario applies to you, finish the initial onboarding workflow for the project in Xcode and connect the instance that hosts your app’s code, then let the first build fail. After the build failure, Xcode suggests a fix to connect the other instance.

### [Review third-party dependencies](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Review-third-party-dependencies)

If you use a third-party dependency manager like [CocoaPods](https://cocoapods.org/) or [Carthage](https://github.com/Carthage/Carthage), or require an additional tool to successfully build your project, you’ll need to make changes to your project or workspace before you can use Xcode Cloud.

Because third-party tools and dependencies require additional work, review and simplify your third-party dependencies before you configure your project or workspace to use Xcode Cloud. For example, you may be able to replace a dependency with a framework that Apple provides. Alternatively, see if its creator offers the dependency as a Swift package. If so, you can use the package and take advantage of the support for the Swift Package Manager without configuring Xcode Cloud to use a third-party tool.

If switching to a Swift package dependency or removing a dependency isn’t practical, follow the instructions below to ensure Xcode Cloud can access dependencies and required tooling.

### [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool)

The temporary build environment Xcode Cloud uses to perform a build doesn’t include third-party tools or dependencies. However, it includes [Homebrew](https://brew.sh/), an open source package manager you can use to install additional software. For example, you can use Homebrew to install dependency managers like [CocoaPods](https://cocoapods.org/) or [Carthage](https://github.com/Carthage/Carthage).

To install a tool with Homebrew:

1.  Create a directory next to your Xcode project or workspace and name it `ci_scripts`.
    
2.  Create an executable shell script, name it `ci_post_clone.sh`, and save it in the `ci_scripts` directory. For example, use the Shell Script template in Xcode to create the file, and then make it an executable by running `chmod +x ci_post_clone.sh` in Terminal.
    
3.  Open the custom script in Xcode and add the necessary commands to install a tool with Homebrew.
    
> Note   
You can use custom build scripts to perform a variety of tasks, but you can’t obtain administrator privileges by using sudo.

For more information about custom build scripts, see [Writing custom build scripts](https://developer.apple.com/documentation/xcode/writing-custom-build-scripts).

### [Make CocoaPods dependencies available to Xcode Cloud](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Make-CocoaPods-dependencies-available-to-Xcode-Cloud)

CocoaPods is an open source dependency manager for Apple platforms. However, the temporary build environment that Xcode Cloud uses to perform a build doesn’t come with the tool pre-installed. If you use CocoaPods, first make sure you commit both your `Podfile` and the `Podfile.lock` file. Then, decide between one of the following options:

-   Add the `Pods` directory to your Git repository by committing it.
    
-   Exclude the `Pods` directory from source control by adding it to your `.gitignore` file.
    

If you commit the `Pods` directory and its contents, you won’t need to install CocoaPods to enable Xcode Cloud to build your project or workspace. It’s worth noting, however, that your source code repository takes up more space when adding the `Pods` directory. Additionally, remember that committing binary dependencies can affect the performance of your Git repository. It’s a general issue when using Git and not specific to Xcode Cloud.

> Tip  
If you decide to commit the Pods directory, consider using Git LFS . It’s pre-installed on the temporary build environment Xcode Cloud uses to build your project.



If you choose to exclude the `Pods` directory from source control, you’ll need to install CocoaPods using a custom build script. The benefit, however, is that the source code repository takes up less disk space and doesn’t slow down your Git repository. To install CocoaPods using a custom build script:

1.  Create a post-clone script as described in [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool).
    
2.  Add commands to the script that install CocoaPods with [Homebrew](https://brew.sh/) and that download your CocoaPods dependencies. The following code snippet shows a basic script to achieve this:
    
    ```
    
    #!/bin/sh
    
    
    # Install CocoaPods using Homebrew.
    brew install cocoapods
    
    
    # Install dependencies you manage with CocoaPods.
    pod install
    ```
    

### [Make Carthage dependencies available to Xcode Cloud](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Make-Carthage-dependencies-available-to-Xcode-Cloud)

Carthage is an open source dependency manager for Apple platforms. However, the temporary build environment that Xcode Cloud uses to build your project doesn’t come with the tool pre-installed. To facilitate projects that rely on the `carthage copy-frameworks` command — most projects do — , install Carthage using a custom build script:

1.  Create a post-clone script as described in [사용자 지정 빌드 스크립트를 사용하여 third-party 종속성 또는 도구 설치](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud#Use-a-custom-build-script-to-install-a-third-party-dependency-or-tool).
    
2.  Add the necessary commands to the script to install Carthage using Homebrew and build your Carthage dependencies.