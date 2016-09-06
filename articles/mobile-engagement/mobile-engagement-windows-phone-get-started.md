---
ms.openlocfilehash: a4cd3318f5c6376146956d904d7c378b5b9d4254
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 移动服务为 Windows Phone Silverlight 应用程序"
    description="了解如何使用 Azure 移动合作分析和推式通知 Windows Phone Silverlight 应用程序。"
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="04/30/2015"
    ms.author="piyushjo" />

# 开始使用 Azure 移动服务为 Windows Phone Silverlight 应用程序

> [AZURE.SELECTOR]
- [Windows 世界](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS 的 Obj C](mobile-engagement-ios-get-started.md)
- [iOS 的 Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

本主题演示如何使用 Azure 移动合作来了解您的应用程序的使用情况和分段的 Windows Phone Silverlight 应用程序的用户发送推式通知。
本教程演示如何使用移动服务的简单广播的方案。 中，您可以创建一个空白的 Windows Phone Silverlight 应用程序收集基本数据和接收推式通知使用 Microsoft 推送通知服务 (MPNS)。 完成本教程后，您将能够广播到所有设备或特定于目标的用户根据其设备属性 （使用 MPNS） 推式通知。 请务必按照下一个教程以查看如何使用移动服务来满足特定用户和组的设备。

> [AZURE.NOTE] 如果您面向 Windows Phone 8.1 (非 Silverlight)，请参阅[Windows 通用教程](mobile-engagement-windows-store-dotnet-get-started.md)。

本教程要求如下︰

+ Visual Studio 2013 年
+ [移动服务的 Windows Phone SDK]

> [AZURE.IMPORTANT] 学完本教程是为 Windows Phone Silverlight 应用程序中，所有其他移动服务教程的先决条件，要完成它，必须有一个活动的 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

##<a id="setup-azme"></a>为您的 Windows Phone Silverlight 应用程序设置移动服务

1. 登录到 Azure 的门户，然后单击**+ 新建**在屏幕的底部。

2. 单击**服务应用程序**，单击**移动服务**，然后单击**创建**。

    ![][7]

3. 在弹出窗口中显示，请输入以下信息︰

    ![][8]

  - **应用程序名称**︰ 键入您的应用程序的名称。 随意使用任何字符。
  - **平台**︰ 选择应用程序的目标平台 (**Windows Phone Silverlight**) （如果您的应用程序以多个平台为目标，请重复本教程中为每个平台）。
 - **应用程序资源名称**︰ 这是依据将是通过 Api 和 Url 可访问此应用程序的名称。 您必须只使用常规 URL 字符。 自动生成的名称应提供强大的基础。 您还应追加要避免任何名称冲突，因为该名称必须是唯一的平台名称。
 - **位置**︰ 选择在其中承载此应用程序 （和更重要的是其集合） 的数据中心。
 - **集合**︰ 如果您已经创建了一个应用程序，选择以前创建的集合;否则，请选择**新的收藏集**。
 - **收藏集名称**︰ 这表示您的应用程序的组。 它还可以确保所有应用程序都允许聚合的计算的度量值组中。 您应该使用您的公司名称或部门这里的话。

4. 选择您刚创建的**应用程序**选项卡中的应用程序。

5. 单击要显示的连接设置以放入您的 SDK 集成在您的移动应用程序中**的连接信息**。

    ![][10]

6. 将**连接字符串**复制-这是您需要确定此应用程序在应用程序代码中的并从您的电话应用程序连接到移动服务。

    ![][11]

##<a id="connecting-app"></a>将您的应用程序连接到移动服务后端

本教程介绍了"基本集成"，这是收集数据和发送推式通知所需的最小集。 [移动服务的 Windows Phone SDK]文档中找不到完整集成文档。

与 Visual Studio 集成的演示，我们将创建一个基本的应用程序。

###创建新的 Windows Phone Silverlight 项目

1. 启动 Visual Studio，并在**主页**屏幕中，选择**新的项目**。

2. 在弹出窗口中，选择**应用商店应用程序** -> **Windows Phone 应用程序** -> **为空的应用程序 (Windows Phone Silverlight)**。 填充应用程序中`Name`， `Solution name`，然后单击**确定**。

    ![][13]

3. 您可以选择**Windows Phone 8.0**或**Windows Phone 8.1**的目标。

现在，您已创建新 Windows Phone Silverlight 应用程序在其中我们将集成 Azure 移动服务 SDK。

###将您的应用程序连接到移动服务后端

1. 安装在您的项目中[移动合作 Windows Phone SDK] nuget 程序包。

2. 打开`WMAppManifest.xml`（在属性文件夹中），请确保以下声明 （添加它们如果不） 在`<Capabilities />`标记︰

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][20]

3. 现在粘贴先前复制为您的移动服务应用程序的连接字符串，并将其粘贴`Resources\EngagementConfiguration.xml`文件，介于`<connectionString>`和`</connectionString>`标记︰

    ![][21]

4. 在`App.xaml.cs`文件︰

    一。 添加`using`语句︰

            using Microsoft.Azure.Engagement;

    b。 初始化在 SDK`Application_Launching`方法︰

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c。 插入以下`Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>启用实时监视

要开始发送数据，并确保用户处于活动状态，您必须发送到移动服务后端的至少一个屏幕 （活动）。 我们将通过创建子类来实现此我们`MainPage`与`EngagementPage`，其移动服务 SDK 提供了。

1. 添加`using`语句︰

       using Microsoft.Azure.Engagement;

2. 将替换超类的**主页**，这是在**PhoneApplicationPage**， **EngagementPage**，与之前，如下所示︰

    ![][22]

3. 在您`MainPage.xml`文件︰

    一。 将添加到命名空间声明︰

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b。 更换`phone:PhoneApplicationPage`中使用的 XML 标记名称`engagement:EngagementPage`。

###确保您的应用程序使用实时监视连接

本节说明如何确保您的应用程序连接到移动服务后端通过移动服务的实时监控功能。

1. 导航到您的移动服务门户。

    从 Azure 门户网站，请确保您是在应用程序中我们使用此项目，并且然后单击底部的**接洽**按钮。

    ![][26]

2. 将您的应用程序的停放服务门户的**设置**页中。 选项卡中单击**监视器**选项卡，如下所示。
    ![][30]

3. 监视器就可以实时地将启动您的应用程序中显示任何设备。

4. 在 Visual Studio，启动您的应用程序在仿真程序中或在已连接的设备。

5. 它是否正常运行，您现在应该看到一个会话监视器中 ！
    ![][33]

**祝贺您 ！** 您成功地完成本教程中使用连接到已发送数据移动服务后端应用程序的第一步。

##<a id="integrate-push"></a>启用推式通知和消息传递应用程序中

移动服务可以进行交互和用户推送通知和市场活动的上下文中的应用程序在消息到达。 本模块调用到达移动服务门户中。
以下各节将您的应用程序设置为接收它们。

###使您的应用程序接收 MPNS 推送通知

添加新的功能，为您`WMAppManifest.xml`文件︰

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][34]

###初始化到达 SDK

1. 在`App.xaml.cs`，调用`EngagementReach.Instance.Init();`在**Application_Launching**函数中，右后代理初始化︰

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. 在`App.xaml.cs`，调用`EngagementReach.Instance.OnActivated(e);`在**Application_Activated**函数中，右后代理初始化︰

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

您已经全部设置。 现在我们将验证已正确 cried 出此基本集成。

##<a id="send"></a>将通知发送到您的应用程序

我们现在将创建简单的推式通知市场到我们的应用程序发送推式通知。

1. 导航到移动服务门户中的**到达**选项卡。

2. 单击**新通知**来创建推入市场。
    ![][35]

3. 设置市场活动执行下面的步骤的第一个字段︰ ![][36]

    1. 使用您喜欢的任何名称命名您的市场活动。
    2. 作为**交货时间**，选择允许应用程序接收的通知是否或不启动该应用程序的*任何时候*。
    3. 在通知文本，键入标题，它将处于推中以粗体显示。
    4. 然后键入您的消息。

4. 向下滚动，然后在**内容**部分中，选择**仅通知**。
    ![][37]

5. 在完成设置最基本的市场活动的可能。 现在再次向下滚动，然后单击**创建**按钮以保存您的市场活动。

6. 最后步骤︰ 单击**激活**来激活您的市场活动并发送推式通知。
![][39]

7. 现在您应该看到一个通知上您的设备，**祝贺 ！**:
![][40]

<!-- URLs. -->
[移动服务的 Windows Phone SDK]: http://go.microsoft.com/?linkid=9874664\[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[7]: ./media/mobile-engagement-windows-phone-get-started/create-mobile-engagement-app.png
[8]: ./media/mobile-engagement-windows-phone-get-started/create-azme-popup.png
[10]: ./media/mobile-engagement-windows-phone-get-started/app-main-page-select-connection-info.png
[11]: ./media/mobile-engagement-windows-phone-get-started/app-connection-info-page.png
[13]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[20]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[21]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[22]: ./media/mobile-engagement-windows-phone-get-started/subclassing.png
[26]: ./media/mobile-engagement-windows-phone-get-started/engage-button.png
[30]: ./media/mobile-engagement-windows-phone-get-started/clic-monitor-tab.png
[33]: ./media/mobile-engagement-windows-phone-get-started/monitor.png
[34]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[35]: ./media/mobile-engagement-windows-phone-get-started/new-announcement.png
[36]: ./media/mobile-engagement-windows-phone-get-started/campaign-first-params.png
[37]: ./media/mobile-engagement-windows-phone-get-started/campaign-content.png
[39]: ./media/mobile-engagement-windows-phone-get-started/campaign-activate.png
[40]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png

测试
