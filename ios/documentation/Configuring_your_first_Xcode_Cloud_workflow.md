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

-    자동화된 UI 테스트에서 생성한 스크린샷을 포함한 테스트 결과 번들 


Xcode를 사용하여 초기에 Xcode Cloud를 사용하도록 프로젝트 또는 workspace를 구성하세요.  
첫번째 빌드를 시작한 후 Xcode 또는 App Store Connect를 사용하여 추가 workflow를 구성하고 빌드 정보에 액세스하는 등의 작업을 수행할 수있습니다.
자세한 내용은 [Edit and create workflows in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Edit-and-create-workflows-in-App-Store-Connect). 를 참고하세요.


### [아카이브 작업 선택](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Select-the-archive-action)

For each app or framework that you want to build with Xcode Cloud, make sure its corresponding scheme uses the archive action:

![A screenshot that shows a scheme of the Fruta app with an enabled archive action.](https://docs-assets.developer.apple.com/published/e945f8514d710390e42820ba221438d6/Configuring-Your-First-Xcode-Cloud-Workflow-1@2x.png)

To find out which of your project’s schemes use the archive action, run the following command in Terminal:

```

xcodebuild -project Example.xcodeproj -describeAllArchivableProducts -json
```

Xcode uses the same command to find available products that you can build with Xcode Cloud.

### [Choose a product](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Choose-a-product)

To configure your project or workspace to use Xcode Cloud, open your project or workspace in Xcode. Make sure the Report navigator is visible, select the Cloud tab, and then click Create Workflow.

![A screenshot of Xcode that shows the Report navigator and the Cloud tab for a project that isn’t using Xcode Cloud.](https://docs-assets.developer.apple.com/published/bf5e5a2b8f28d0e81c0e83addac97313/Configuring-Your-First-Xcode-Cloud-Workflow-2@2x.png)

Xcode analyzes your project or workspace and lists each product it finds in the Select a Product sheet. Choose the product that matches your app or framework and click Next.

![A screenshot of the Select a Product sheet in Xcode that lists all products for the Fruta app.](https://docs-assets.developer.apple.com/published/07ee5eb73ec5e84c53b4716d0a544368/Configuring-Your-First-Xcode-Cloud-Workflow-3@2x.png)

If your project contains targets that use the same bundle identifier, Xcode Cloud considers them to be one product. Note that a product can only have one bundle ID, and a bundle ID always matches exactly one Xcode Cloud product. In case your workspace or project contains several app targets:

-   When possible, share the same bundle ID across versions of your app for each platform. For example, use the same bundle ID for your iOS, macOS, and watchOS versions of your app.
    
-   If each version of your app uses a different bundle ID — for example, if the iOS version uses `com.example.myiosapp` and the macOS app uses `com.example.mymacapp` — Xcode detects more than one product. Choose one of them when you create your first workflow, and later configure additional workflows for the other product.
    

You may be a member of more than one Apple Developer team. In this case, Xcode asks you to choose a team. Choose the team you intend to use for distribution to testers with TestFlight and for publishing the app in the App Store.

### [Review the suggested workflow](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Review-the-suggested-workflow)

Based on the product you selected, Xcode suggests a first workflow that:

-   Starts a build for each change to your Git repository’s default branch and for each pull request that targets your default branch
    
-   Uses the latest released macOS and Xcode versions for its temporary build environment
    
-   Uses the archive action
    
-   Sends an email with information about the build, including links to the build report in Xcode and App Store Connect
    

Before starting your first build, review the suggested workflow; for example, verify if Xcode chose the correct scheme.

To review the suggested workflow:

1.  Click Edit Workflow in the Review Workflow sheet.
    
2.  Make changes only if necessary in the sheet that displays the workflow information and save them. ![A screenshot of the suggested workflow in Xcode for the Fruta app.](https://docs-assets.developer.apple.com/published/f00fe5c3b5dd12f29978feca81c98da4/Configuring-Your-First-Xcode-Cloud-Workflow-4@2x.png)
    
3.  In the Review Workflow sheet, click Next and let Xcode guide you through the process of granting Xcode Cloud access to your source code.
    

### [Grant Xcode Cloud access to your source code](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Grant-Xcode-Cloud-access-to-your-source-code)

Xcode Cloud requires access to the Git repository containing your code. It uses this access to build and test your code automatically when you make changes. When you configure your project or workspace to use Xcode Cloud, Xcode analyzes it to detect the Source Code Management (SCM) provider you use. In the Grant Access to Your Source Code sheet, click Grant Access and let Xcode guide you through your SCM provider’s native authorization process.

![A screenshot of the Grant Access to Your Source Code sheet in Xcode.](https://docs-assets.developer.apple.com/published/370da2c2fcf558030fd8b44500dac3b6/Configuring-Your-First-Xcode-Cloud-Workflow-5@2x.png)

When you’ve allowed Xcode Cloud to access your Git repository, Xcode indicates that it can access your source code; click Next.

For additional guidance on granting Xcode Cloud access to your source code, see [Source code management setup](https://developer.apple.com/documentation/xcode/source-code-management-setup).

### [Create an app record](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Create-an-app-record)

Xcode Cloud combines Xcode, [TestFlight](https://developer.apple.com/testflight/), and App Store Connect into a powerful CI/CD system. As a result, you need an app record in App Store Connect for your app.

If you previously created an app record in App Store Connect for your app, Xcode Cloud uses it automatically. If you haven’t created an app record for your app, Xcode helps you create one after you grant Xcode Cloud access to your Git repository.

To create an app record, you must have the App Manager, Admin, or Account Holder role for your Apple Developer Team. If you have the Developer role, you need the Create Apps permission. If you don’t have the required role or permission, see [Create an app record in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Create-an-app-record-in-App-Store-Connect).

### [Start your first build](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Start-your-first-build)

Now that you’ve granted Xcode Cloud access to your Git repository and — if applicable — created an app record, you’re ready to start your first build. Choose a branch and click Start Build. Xcode Cloud checks out the branch and starts building your code.

![A screenshot of the Start Build sheet in Xcode for the Fruta app.](https://docs-assets.developer.apple.com/published/7450d7c337f31cf4f7a578035b6be55c/Configuring-Your-First-Xcode-Cloud-Workflow-6@2x.png)

To view information about the in-progress build in the Editor pane, select the build in the Report navigator. To see detailed build logs, expand an action in the Report Outline and select Logs. If the Report Outline isn’t visible, enable it using the Adjust Editor Options button in the top-right corner of the Editor pane.

When Xcode Cloud finishes building your project, it sends an email that contains information about the build: the build’s status, the commit it used for the build, and links to the build report in Xcode or in App Store Connect.

### [Understand why a build failed](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Understand-why-a-build-failed)

There’s a chance that your first build will fail. This is especially likely for complex code bases and projects with many dependencies. To understand why a build failed, select a build in the Report navigator, expand a failed action in the Report Outline, and choose Logs to see the build logs.

![A screenshot of Xcode that shows detailed build information in the Editor pane for a failed archive action.](https://docs-assets.developer.apple.com/published/ba9dafa2ed0f6e806ddcbfc73e585049/Configuring-Your-First-Xcode-Cloud-Workflow-7@2x.png)

You can also choose Artifacts and download the build report. For additional guidance on fixing common build issues, see [Resolving common configuration and build issues](https://developer.apple.com/documentation/xcode/resolving-common-configuration-and-build-issues).

In addition to viewing build logs in Xcode, you can also explore build logs in App Store Connect.

To see build logs in App Store Connect:

1.  Log in to App Store Connect and go to your app’s page.
    
2.  Select the Xcode Cloud tab.
    
3.  Choose Builds in the sidebar.
    
4.  Expand a workflow and choose a build.
    
5.  Expand an action in the sidebar and choose Logs.
    

### [Refine your continuous integration practice](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Refine-your-continuous-integration-practice)

After configuring your first workflow and successfully completing your first build, spend time planning next steps to refine your CI/CD process; for example:

-   Ask coworkers to start using Xcode Cloud as described in [Connect your personal SCM account to Xcode Cloud](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Connect-your-personal-SCM-account-to-Xcode-Cloud).
    
-   Change your first workflow’s name and description.
    
-   Add a test action to your first workflow that runs your unit tests.
    
-   Change your first workflow’s start conditions to only start a build if you update a custom branch or add start conditions.
    
-   Add a post-action to distribute a new version of your app to testers with [TestFlight](https://developer.apple.com/testflight/).
    
-   Create additional workflows to perform advanced verifications that take more time to complete; for example, configure a workflow that runs your automated UI tests once per week.
    
-   Create workflows for other products that Xcode detected when you created your first workflow.
    
-   Receive build information in [Slack](https://slackhq.com/), a popular collaboration tool. For information about connecting Xcode Cloud to Slack, see [Connecting Xcode Cloud to Slack](https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-slack).
    
-   Require an Xcode Cloud build to succeed before it’s possible to merge a PR. For more information, see [Configuring requirements for merging a pull request](https://developer.apple.com/documentation/xcode/configuring-requirements-for-merging-a-pull-request).
    

### [Create additional workflows in Xcode](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Create-additional-workflows-in-Xcode)

To configure and create additional workflows or make changes to an existing one in Xcode:

1.  Navigate to the Cloud tab in the Report navigator.
    
2.  Control-click your app’s name or a workflow and choose Manage Workflows.
    
3.  In the Manage Workflows sheet, double-click a workflow to make changes to it or add a new workflow using the Add button (+).
    

For more information on creating custom workflows, see [Developing a workflow strategy for Xcode Cloud](https://developer.apple.com/documentation/xcode/developing-a-workflow-strategy-for-xcode-cloud) and [Xcode Cloud workflow reference](https://developer.apple.com/documentation/xcode/xcode-cloud-workflow-reference).

### [Edit and create workflows in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Edit-and-create-workflows-in-App-Store-Connect)

You must configure your first Xcode Cloud workflow in Xcode, and the deep integration of Xcode Cloud into Xcode enables an integrated development process where you write code, review changes, view build information, and configure workflows. However, teams may have dedicated infrastructure engineers or release managers who aren’t familiar with Xcode. To accommodate them, and to offer you an additional way to configure your workflows and view build information, App Store Connect also deeply integrates with Xcode Cloud.

To view, edit, or create workflows in App Store Connect:

1.  Log in to App Store Connect and go to your app’s page.
    
2.  Click the Xcode Cloud tab.
    
3.  Choose Manage Workflows in the sidebar.
    
4.  Click the Add button next to Manage Workflows to create a new workflow or click a workflow to view and edit its settings.
    

By clicking the More button (···) for a workflow, you can also edit, duplicate, deactivate, or delete a workflow.

### [Download and archive build artifacts](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Download-and-archive-build-artifacts)

When Xcode Cloud completes a build for a workflow, it creates a set of artifacts that includes build information, app binaries, symbol information, test results, and more. Xcode Cloud stores artifacts for up to 30 days after it completes a build.

Beyond the need to archive past builds, it’s especially important to download and archive build artifacts for a version of an app that you distribute on the App Store. This is because you may need the symbol information Xcode Cloud creates when it archives your app to diagnose issues using crash reports.

To download build information and artifacts, use Xcode or App Store Connect. Alternatively, use the App Store Connect API to automate the task of downloading the build artifacts.

For information on automating Xcode Cloud with the App Store Connect API, see [Xcode Cloud Workflows and Builds](https://developer.apple.com/documentation/appstoreconnectapi/xcode_cloud_workflows_and_builds). For information on symbol information and crash reports, see [Diagnosing issues using crash reports and device logs](https://developer.apple.com/documentation/xcode/diagnosing-issues-using-crash-reports-and-device-logs).