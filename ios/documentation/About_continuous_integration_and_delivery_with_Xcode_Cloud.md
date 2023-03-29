# Xcode Cloud를 통한 지속적 통합 및 배포 정보
### Xcode Cloud를 사용한 지속적 통합 및 배포가 어떻게 고품질의 앱과 프레임워크를 만드는데 도움이 되는지 배울수 있습니다.


<br>
<br>

## [Overview](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Overview)

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

You don’t need to introduce every aspect of continuous integration and delivery at once. Instead, take a slow but steady approach and start by using [Git](https://git-scm.com/) for managing your project’s source code. After you’re comfortable with Git’s workflows and processes, configure your project to use Xcode Cloud at the most basic level — for continually building and testing your projects. As you become more familiar with how Xcode Cloud works, you can start to take advantage of its CD features like delivering test builds with TestFlight.

### [The role of source control](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#The-role-of-source-control)

Changes to your app’s code can be difficult to manage. This is especially true when you work on several changes at the same time. To help you organize, manage, and document source code changes, Xcode supports source control with Git. By using source control, you improve your project’s quality by tracking and reviewing code changes.

Because source control plays such a fundamental role in CI/CD, Xcode Cloud requires your code to be in a Git repository. It supports the following _source code management_ (SCM) providers:

-   [Bitbucket Cloud](https://bitbucket.org/) and [Bitbucket Server](https://bitbucket.org/product/enterprise)
    
-   [GitHub](https://github.com/) and [GitHub Enterprise](https://github.com/enterprise)
    
-   [GitLab](https://gitlab.com/) and [self-managed GitLab](https://about.gitlab.com/install)
    

To learn more about source control with Xcode, see [Source control management](https://developer.apple.com/documentation/xcode/source-control-management).

### [The importance of automated building and testing](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#The-importance-of-automated-building-and-testing)

A typical development process starts with making changes to your code, building the project, and running your app in the Simulator or on a test device. Your process may even include verifying changes locally by running unit tests you create with [XCTest](https://developer.apple.com/documentation/xctest), and by running integration, performance, or user interface tests.

While Xcode can reduce the time it takes to perform these tasks, they still take a significant amount of your time. This is especially true for complex apps or frameworks that support a wide range of devices and operating systems. With Xcode Cloud, however, you can build, run, and test your project on multiple simulated devices in less time than you can traditionally.

![A figure that illustrates the various steps of automated building and testing: a change leads to automated building, testing, and other actions that verify a change.](https://docs-assets.developer.apple.com/published/2427429f22f500f2ccc63735bf2e77bc/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-2@2x.png)

After verifying a change, Xcode Cloud automatically notifies you about the result via email. You can also view the results in Xcode or in App Store Connect. This helps you detect errors before they become an issue, and gives you the reassurance you need about the stability and quality of your code.

For more information about automatically building your project with Xcode Cloud, see [Configuring your first Xcode Cloud workflow](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow).

For more information about testing your code, see [Testing your apps in Xcode](https://developer.apple.com/documentation/xcode/testing-your-apps-in-xcode) and [Adding unit tests to your existing project](https://developer.apple.com/documentation/xcode/adding-unit-tests-to-your-existing-project).

### [Continuous delivery](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Continuous-delivery)

The flip side to continuous integration (CI) is continuous delivery (CD). While automatic building and testing are important to developing high-quality apps and frameworks, it doesn’t replace the hands-on testing you get from team members and testers. When Xcode Cloud verifies a change to your code (CI), it can automatically deliver (CD) a new version of your app to internal testers with [TestFlight](https://developer.apple.com/testflight/). Xcode Cloud can also sign your app for external testing with TestFlight and submission to app review. You can also upload the exported app archive or a framework to your own server.

![A figure that illustrates the various steps of CD that happen after CI has passed: distribution to internal testers, external testers, and the App Store.](https://docs-assets.developer.apple.com/published/35478ba6fa547f5adb9387b9e78b99b6/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-3@2x.png)

### [Collaborative software development with Xcode Cloud](https://developer.apple.com/documentation/xcode/about-continuous-integration-and-delivery-with-xcode-cloud#Collaborative-software-development-with-Xcode-Cloud)

Source control with Git helps you manage changes to your code. For example, Git branches enable you to make a change without affecting your verified, stable codebase. Additionally, branches are helpful when you develop your app or framework as a team. However, merging changes, resolving conflicts, and verifying code changes can be time-consuming.

To make code review and merging easier, SCM providers offer support for _pull requests_ (PRs), also known as _merge requests_. When you create a PR, you inform your team that a change is ready for review, and you and your team can take a look at each other’s code changes, and provide and address feedback before merging branches.

![A figure that illustrates a collaborative software development workflow with Git. A team member creates a PR, gathers feedback, addresses the feedback, and eventually merges branches.](https://docs-assets.developer.apple.com/published/352978554e67a633b443c9a585695f1f/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-4@2x.png)

If you develop code as a team, or if you work on several changes at the same time, a common practice to follow is to use separate branches for each change and then create PRs to receive code reviews from your peers. This introduces another verification step that helps you identify errors before they become serious issues.

Xcode allows you to create, view, and comment on PRs, and merge changes into your codebase if you host your Git repository with [Bitbucket Server](https://bitbucket.org/product/enterprise), [GitHub](https://github.com/), or [GitHub Enterprise](https://github.com/enterprise).

![A figure that illustrates how Xcode Cloud integrates with pull requests. When someone creates a pull request, Xcode Cloud detects this change, builds the project, runs configured tests, and posts the status to the pull request.](https://docs-assets.developer.apple.com/published/b158559eb1ebf931b65b806796cc8445/About-Continuous-Integration-and-Delivery-with-Xcode-Cloud-5@2x.png)

You can also configure Xcode Cloud to detect new PRs, or changes to an existing PR. When it detects a change, Xcode Cloud merges the involved branches in a temporary build environment, and automatically builds your project and runs your tests to verify the merged code. After verifying the changes, Xcode Cloud adds a status message to the PR to inform you of the result.