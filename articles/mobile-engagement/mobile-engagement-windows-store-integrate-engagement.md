---
ms.openlocfilehash: 9cca6e6a497289af09e73f84ca7505adcb87e01b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Windows 应用程序的通用接洽 SDK 集成" 
    description="如何与 Windows 通用应用程序集成 Azure 的移动服务"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="piyushjo" />

#Windows 应用程序的通用接洽 SDK 集成

> [AZURE.SELECTOR] 
- [通用的 Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

本过程描述了最简单的方法来激活服务活动的分析和监视 Windows 通用应用程序中的功能。

以下步骤是不足以激活计算有关用户、 会话、 活动、 系统崩溃和 Technicals 的所有统计信息所需的日志报告。 必须手动使用服务 API 完成计算事件、 错误和作业等其他统计数据所需的日志报告 （由于这些统计信息是依赖的应用程序，请参阅[如何使用高级的移动服务标记 API 通用 Windows 应用程序中的](mobile-engagement-windows-store-use-engagement-api.md)。

##受支持的版本

移动服务 SDK 的 Windows 通用应用程序只可以集成到面向 Windows 运行时应用程序︰

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1

> [AZURE.NOTE] 如果针对 Windows Phone 8.1 Silverlight 参考[Windows Phone Silverlight 集成过程](mobile-engagement-windows-phone-integrate-engagement.md)。


##安装移动签约通用应用程序 SDK

移动服务 SDK 的 Windows 通用应用程序是可用 Nuget 程序包称为*MicrosoftAzure.MobileEngagement*。 您可以安装的 Visual Studio Nuget 程序包管理器。 

##添加功能

接洽 SDK 需要 Windows SDK 的某些功能才能正常工作。

打开您`Package.appxmanifest`文件，并确保都声明了以下功能︰

-   `Internet (Client)`

##初始化服务 SDK

### 服务配置

服务配置集中在`Resources\EngagementConfiguration.xml`的项目文件。

编辑此文件，以指定︰

-   应用程序连接字符串标记之间`<connectionString>`， `<\connectionString>`。

如果您想要指定它在运行时，可以调用下面的方法在签约代理初始化之前︰
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

        #if WINDOWS_PHONE_APP
          /* Connection string for my Windows Phone App. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        #else
          /* Connection string for my Windows Store App. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        #endif

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

在 Azure 门户网站上显示您的应用程序的连接字符串。

> [AZURE.WARNING] 您不需要使用条件编译符号`WINDOWS_PHONE_APP`为您只需在一个平台上独立 Windows 运行时应用程序定义不同的配置。

### 服务初始化

当您创建新项目时，`App.xaml.cs`生成的文件。 此类继承自`Application`，包含很多重要的方法。 它还将用于初始化接洽 SDK。

修改`App.xaml.cs`:

-   将添加到您`using`语句︰

        using Microsoft.Azure.Engagement;

-   插入`EngagementAgent.Instance.Init`在`OnLaunched`方法︰

        protected override void OnLaunched(LaunchActivatedEventArgs args)
        {
          EngagementAgent.Instance.Init(args);
        
          // or
        
          EngagementAgent.Instance.Init(args, engagementConfiguration);
        }

-   使用自定义方案、 另一个应用程序或命令行启动时应用程序然后`OnActivated`调用方法。 您还需要启动签约代理，您的应用程序被激活时。 为此，请重写`OnActivated`方法︰

        protected override void OnActivated(IActivatedEventArgs args)
        {
          EngagementAgent.Instance.Init(args);
        
          // or
        
          EngagementAgent.Instance.Init(args, engagementConfiguration);
        }

> [AZURE.IMPORTANT] 我们强烈建议您在您的应用程序的另一个位置添加服务初始化。

##基本报告

### 推荐的方法︰ 重载您`Page`类

为了激活接洽，以计算用户、 会话、 活动、 系统崩溃和技术统计数据所需的所有日志的报告，您可以只是使所有您`Page`子类继承`EngagementPage`类。

这里是如何为您的应用程序页执行此操作的示例。 您可以执行相同的操作，您的应用程序的所有页。

#### C# 源文件

修改您的网页`.xaml.cs`文件︰

-   将添加到您`using`语句︰

        using Microsoft.Azure.Engagement;

-   更换`Page`与`EngagementPage`:

**而无需接洽︰**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**与合作︰**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] 如果您的页面重写`OnNavigatedTo`方法时，请务必调用`base.OnNavigatedTo(e)`。 否则，将不报告活动 (`EngagementPage`调用`StartActivity`在其`OnNavigatedTo`方法)。

#### XAML 文件

修改您的网页`.xaml`文件︰

-   将添加到命名空间声明︰

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   更换`Page`与`engagement:EngagementPage`:

**而无需接洽︰**

        <Page>
            <!-- layout -->
            ...
        </Page>

**与合作︰**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### 重写默认行为

默认情况下，页面的类名称被报告为活动名称，并带有无需另付。 如果类使用"页面"后缀，合作也将删除它。

如果您想要重写默认行为的名称，只要将它添加到您的代码︰

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

如果您要报告一些与您的活动的额外信息，您可以向代码中添加此︰

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

这些方法称为从`OnNavigatedTo`页面的方法。

### 备选方法︰ 调用`StartActivity()`手动

如果您不能或不想重载您`Page`的类，而是可以通过调用来启动您的活动`EngagementAgent`直接方法。

我们建议您调用`StartActivity`内您`OnNavigatedTo`页面的方法。

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  确保正确地结束您的会话。
> 
> 自动调用 Windows 通用 SDK`EndActivity`时关闭该应用程序的方法。 因此，**强烈**建议您调用是`StartActivity`方法，只要该活动的用户的更改，并**从不**调用`EndActivity`方法，此方法将发送到服务服务器，当前用户已将该应用程序，这将会影响所有的应用程序日志。

##高级报告

（可选） 您可以对报表应用程序特定事件、 错误和作业，若要执行此操作，使用其他方法中找到`EngagementAgent`类。 服务 API 允许使用所有服务活动的高级功能。

有关详细信息，请参阅[如何使用高级的移动服务标记 Windows 通用应用程序中的 API](../mobile-engagement-windows-store-use-engagement-api/)。

##高级的配置

### 禁用自动故障报告

您可以禁用自动崩溃报告服务的功能。 然后，将在发生未经处理的异常时合作将不会任何操作。

> [AZURE.WARNING] 如果要禁用此功能，请注意，当未经处理的崩溃将发生在您的应用程序中，服务将发送崩溃**并**不会关闭会话和作业。

若要禁用自动崩溃报告，只是自定义根据声明的方式配置︰

#### 从`EngagementConfiguration.xml`文件

将报告故障设置为`false`之间`<reportCrash>`和`</reportCrash>`标记。

#### 从`EngagementConfiguration`在运行时的对象

设置为 false，使用 EngagementConfiguration 对象报告故障。

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### 突发模式

默认情况下，接洽服务报告实时记录。 如果您的应用程序非常频繁地报告日志，最好是缓冲的日志和报告他们一次性全部按时正则基 （这称为"突发模式"）。

若要执行此操作，调用该方法︰

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

该参数是以**毫秒为单位**的值。 在任何时候，如果想要重新激活的实时记录，只需调用的方法不带任何参数，或使用 0 值。

突发模式略有延长电池寿命，但接洽显示器上有一定影响︰ 所有的会话和作业工期将舍入为突发事件阈值 （因此，会话和作业比可能不会爆发阈值的短）。 建议将突发事件阈值不超过 30000 （30 秒）。 您一定要注意保存日志仅限于 300 项。 如果发送过长可能会丢失某些日志。

> [AZURE.WARNING] 突发事件阈值不能配置为小于 1 的时间段。 如果您尝试这样做，SDK 将显示带有错误的跟踪，并且将自动重置为默认值，即 0。 该操作将触发 SDK 中实时的日志报告。

[此处]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet 网站]:http://docs.nuget.org/docs/start-here/overview
 

测试
