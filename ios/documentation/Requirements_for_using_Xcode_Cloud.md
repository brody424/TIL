# Xcode Cloud 사용을 위한 요구 사항
### Xcode Cloud를 사용하려면 프로젝트 또는 작업 공간(workspace)을 구성하기 전에 계정, 프로젝트 및 소스 제어 요구 사항을 검토하세요.


<br>
<br>

## [Overview](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Overview)
---  

Xcode는 Xcode Cloud를 사용하도록 프로젝트 또는 작업공간을 구성하는데 도움이 됩니다.  
원할한 프로세스를 위해 Xcode Cloud 사용에 대한 요구 사항을 검토하고 필요에 따라 변경합니다.  
그런 다음 Xcode Cloud를 사용하도록 프로젝트 또는 작업 공간을 구성하고 CI/CD(지속적인 통합 및 배포) 연습을 시작하세요.

### [Developer account requirements](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Developer-account-requirements)

To use Xcode Cloud, you must:

-   Be enrolled in the [Apple Developer Program](https://developer.apple.com/programs/).
    
-   Add your Apple ID under Accounts in Xcode settings.
    
-   Have an app record for your app in [App Store Connect](https://appstoreconnect.apple.com/) or have the required role or permission to create one.  

<br>
    
> Note  
 To create an app record, you must have the App Manager, Admin, or Account Holder role for your Apple Developer team. If you have  the Developer role, you need the Create Apps permission. If you don’t have the required role or permission, work with a team member who does. For more information, see [Create an app record in App Store Connect](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Create-an-app-record-in-App-Store-Connect).


For more information about roles in App Store Connect, see [Role permissions](https://developer.apple.com/help/app-store-connect/reference/role-permissions). For more information about creating an app record, see [Add a new app](https://developer.apple.com/help/app-store-connect/create-an-app-record/add-a-new-app).

### [Project and workspace requirements](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Project-and-workspace-requirements)

To use Xcode Cloud, be sure to meet the following project and workspace requirements:

-   You use a consistent Xcode project or workspace.
    
-   You use shared schemes. For information on sharing a scheme, see [Customizing the build schemes for a project](https://developer.apple.com/documentation/xcode/customizing-the-build-schemes-for-a-project).
    
-   You’ve enabled the archive action for the scheme that builds your app or framework.
    
-   You use Xcode’s new build system. For more information about the new build system, see [Build System Release Notes for Xcode 10](https://developer.apple.com/documentation/Xcode-Release-Notes/build-system-release-notes-for-xcode-10). For details about how to turn on the new build system, see [Choose the build system](https://help.apple.com/xcode/mac/#/dev396bc94c7).
    
-   Your dependencies and additional third-party tools are available to Xcode Cloud. For more information on making tools and dependencies available to Xcode Cloud, see [Making dependencies available to Xcode Cloud](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud).
    
-   You use automatic code signing. To learn more about automatic code signing, see [Signing & Capabilities Workflow](https://help.apple.com/xcode/mac/current/#/dev60b6fbbc7) and [Code Signing](https://developer.apple.com/support/code-signing/).
    
-   You set the bundle identifier for your app target in the Signing & Capabilities tab of your Xcode project or workspace. If you use `.xcconfig` files to set the bundle identifier, see [Review Xcode Cloud workflows](https://developer.apple.com/documentation/xcode/configuring-your-first-xcode-cloud-workflow#Review-Xcode-Cloud-workflows) for more information.
    

### [Source control requirements](https://developer.apple.com/documentation/xcode/requirements-for-using-xcode-cloud#Source-control-requirements)

Using Git for source control is a requirement to use Xcode Cloud. To learn more about using source control with Git in Xcode, see [Source control management](https://developer.apple.com/documentation/xcode/source-control-management).

Xcode Cloud supports the following _source code management_ (SCM) providers:

-   [Bitbucket Cloud](https://bitbucket.org/) and [Bitbucket Server](https://bitbucket.org/product/enterprise)
    
-   [GitHub](https://github.com/) and [GitHub Enterprise](https://github.com/enterprise)
    
-   [GitLab](https://gitlab.com/) and [self-managed GitLab instances](https://about.gitlab.com/install)
    

Additionally, you need a certain permission or role to connect Xcode Cloud to your Git repository. The exact permission depends on the SCM provider you use:

-   If you host your code on [Bitbucket Cloud](https://bitbucket.org/) or [Bitbucket Server](https://bitbucket.org/product/enterprise), you need the _administrator_ permission.
    
-   If you host your code on [GitHub](https://github.com/) or [GitHub Enterprise](https://github.com/enterprise), you need to be an _organization owner_ or need the _admin_ permission if you don’t use a GitHub organization.
    
-   If you host your code on [GitLab](https://gitlab.com/), or on a [self-managed GitLab instance](https://about.gitlab.com/install), you need the _maintainer_ permission.
    

If you don’t have the required role or permission, work with a team member who does. For more information, see [Connect Xcode Cloud to an admin-managed Git repository](https://developer.apple.com/documentation/xcode/configuring-xcode-cloud-for-your-team#Connect-Xcode-Cloud-to-an-admin-managed-Git-repository).