---
ms.openlocfilehash: 0870a72e68e5e591202a8e8420cb203912c49661
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 移动服务为 Windows 通用应用程序"
    description="了解如何使用 Windows 通用应用程序的分析和推式通知的 Azure 移动服务。"
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
    ms.topic="hero-article"
    ms.date="04/30/2015"
    ms.author="piyushjo" />

# 开始使用 Azure 移动服务为 Windows 通用应用程序

> [AZURE.SELECTOR]
- [通用的 Windows](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS |Obj C](mobile-engagement-ios-get-started.md)
- [iOS |Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

本主题演示如何使用 Azure 移动合作来了解您的应用程序的使用情况和分段的 Windows 通用应用程序的用户发送推式通知。
本教程演示如何使用移动服务的简单广播的方案。 您将创建一个空白 Windows 通用应用程序，收集基本的应用程序使用情况数据并接收推式通知使用 Windows 通知服务 (WNS)。 完成本教程后，您将能够广播到所有设备或特定于目标的用户根据其设备属性的推式通知。 请务必按照下一个教程以查看如何使用移动服务来满足特定用户和组的设备。

本教程要求如下︰

+ Visual Studio 2013 年
+ [移动服务窗口通用 SDK]

> [AZURE.IMPORTANT] 学完本教程是为 Windows 通用应用程序的所有其它移动服务指南的先决条件。 若要完成它的必须活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 免费试用版</a>。

##<a id="setup-azme"></a>为您的 Windows 通用应用程序设置移动服务

1. 登录到 Azure 的门户，然后单击**+ 新建**在屏幕的底部。

2. 单击**服务应用程序**，单击**移动服务**，然后单击**创建**。

    ![][7]

3. 在弹出窗口中显示，请输入以下信息︰

    ![][8]

     - **应用程序名称**︰ 键入您的应用程序的名称。 随意使用任何字符。
     - **平台**︰ 选择应用程序的目标平台 （**Windows 通用**） （如果您的应用程序以多个平台为目标，请重复本教程中为每个平台）。
     - **应用程序资源名称**︰ 这是依据将是通过 Api 和 Url 可访问此应用程序的名称。 您必须只使用常规 URL 字符。 自动生成的名称应提供强大的基础。 您还应追加要避免任何名称冲突，因为该名称必须是唯一的平台名称。
     - **位置**︰ 选择在其中承载此应用程序 （和更重要的是其集合） 的数据中心。
     - **集合**︰ 如果您已经创建了一个应用程序，选择以前创建的集合，否则，选择**新的收藏集**。
     - **收藏集名称**︰ 这表示您的应用程序的组。 它还可以确保所有应用程序都允许聚合的计算的度量值组中。 您应该使用您的公司名称或部门这里的话。

    > [AZURE.TIP] 如果通用应用程序要面向 Windows 和 Windows Phone 平台，您仍然应该创建两个移动服务应用程序，另一个用于每个受支持的平台。 这是受众的为了确保您能够创建正确细分并能够发送目标适当地通知每个平台。

4. 选择您刚创建的**应用程序**选项卡中的应用程序。

5. 单击以显示的**连接**设置以放入您的 SDK 集成在您的移动应用程序中的**连接信息**。

    ![][10]

6. 将**连接字符串**复制-这是什么您需要来标识此应用程序在应用程序代码中的，并从通用应用程序的移动合作与交流。

    ![][11]

##<a id="connecting-app"></a>将您的应用程序连接到移动服务后端

本教程介绍了"基本集成，"这是最小的设置需要收集数据，并发送推式通知。 移动服务窗口通用 SDK 文档中找不到完整集成文档。

与 Visual Studio 集成的演示，我们将创建一个基本的应用程序。

###创建新的 Windows 通用应用程序项目

如果您已有一个应用程序，并熟悉 Windows 通用开发，可以跳过此步骤。

1. 启动 Visual Studio，并在**主页**屏幕中，选择**新的项目**。

2. 在弹出窗口中，选择**应用商店应用程序** -> **通用应用程序** -> **为空的应用程序 （通用应用程序）**。 填写的**名称**的应用程序和**解决方案名称**，然后单击**确定**。

    ![][13]

现在已创建新的 Windows 通用应用程序项目在其中我们将集成 Azure 移动服务 SDK。

###将您的应用程序连接到移动服务后端

1. 安装在您的项目中[移动合作 Windows 通用 SDK] nuget 程序包。 如果您的目标窗口和 Windows Phone 平台，需要对这两个项目执行此操作。 相同的 Nuget 程序包将正确的特定于平台的二进制文件放在每个项目。

2. 打开**Package.appxmanifest** ，如果它不会自动添加添加下列代码︰

        Internet (Client)

    ![][20]

3. 现在先复制为您的移动服务应用程序的连接字符串进行复制和粘贴在`Resources\EngagementConfiguration.xml`文件，介于`<connectionString>`和`</connectionString>`标记︰

    ![][22]

    >[AZURE.TIP] 如果您的应用程序是面向 Windows 和 Windows Phone 平台，您仍然应该创建两个移动服务应用程序-一个用于每个受支持的平台。 这是受众的为了确保您能够创建正确细分并能够发送目标适当地通知每个平台。

4. 在`App.xaml.cs`文件︰

    一。 添加`using`语句︰

            using Microsoft.Azure.Engagement;

    b。 初始化的**OnLaunched**方法中的 SDK:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              EngagementAgent.Instance.Init(e);

              //... rest of the code
            }

    c。 在**OnActivated**方法中插入以下并添加方法，如果它不存在︰

            protected override void OnActivated(IActivatedEventArgs e)
            {
              EngagementAgent.Instance.Init(e);

              //... rest of the code
            }

##<a id="monitor"></a>启用实时监视

要开始发送数据，并确保用户处于活动状态，您必须发送到移动服务后端的至少一个屏幕 （活动）。 我们通过设置**EngagementPage**，其移动服务 SDK 提供了与我们**主页**的子类实现这一点。

1.  添加`using`语句︰

           using Microsoft.Azure.Engagement;

2. **主页**，这是在**页面**之前，超级类替换为**EngagementPage**:

    ![][23]

3. 在您`MainPage.xml`文件︰

    一。 将添加到命名空间声明︰

            xmlns:engagement="using:Microsoft.Azure.Engagement"

    b。 **页**中的 XML 标记名称替换**接洽︰ EngagementPage**。
    
> [AZURE.IMPORTANT] 如果您的页面重写`OnNavigatedTo`方法时，请务必调用`base.OnNavigatedTo(e)`。 否则，将不报告活动 (`EngagementPage`调用`StartActivity`在其`OnNavigatedTo`方法)。 这一点尤其重要，Windows Phone 项目中的默认模板，有`OnNavigatedTo`方法。 

###确保您的应用程序使用实时监视连接

本节说明如何确保您的应用程序连接到移动服务后端通过移动服务活动的实时监控功能。

1. 导航到您的移动服务门户。

    从 Azure 门户网站，请确保您是在应用程序中我们使用此项目，并且然后单击底部的**接洽**按钮。

    ![][26]

2. 将您的应用程序的停放服务门户的**设置**页中。 据此，单击**监视器**选项卡，如下所示。
![][30]

3. 监视器就可以实时地将启动您的应用程序中显示任何设备。

4. 在 Visual Studio，启动您的应用程序在仿真程序中或在已连接的设备。

5. 它是否正常运行，您现在应该看到一个会话中实时监视器 ！
![][33]

**祝贺您 ！** 您成功地完成本教程中使用连接到已发送数据移动服务后端应用程序的第一步。

##<a id="integrate-push"></a>启用推式通知和消息传递应用程序中

移动服务允许您进行交互并到达您的用户使用推式通知和 app 市场活动的上下文中的消息。 本模块调用到达移动服务门户中。
以下各节将您的应用程序设置为接收它们。

###使您的应用程序接收 WNS 推送通知

1. 在`Package.appxmanifest`文件，在**应用程序**选项卡下**通知**，选择**是**的**能够祝酒︰**:

    ![][35]

###初始化到达 SDK

1. 在`App.xaml.cs`，代理初始化之后立即调用**OnLaunched**函数中的**EngagementReach.Instance.Init();** :

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

2. 在`App.xaml.cs`，代理初始化之后立即调用**OnActivated**函数中的**EngagementReach.Instance.Init(e);** :

        protected override void OnActivated(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

你设置用于发送祝酒。 现在，我们将会验证您正确执行这一基本集成。

###移动服务发送通知授予访问权限

1. 您需要将您的应用程序与 Windows 应用商店应用程序来获取**包的安全标识符 (SID)**和您**的机密密钥**（客户机密） 相关联。 您可以从[Windows 开发人员中心存储库]创建一个应用程序并请确保为使用**相关联的应用程序与存储**从 Visual Studio。

2. 导航到您的移动服务门户**设置**和单击左侧的**本机推**节。

3. 请单击**编辑**按钮以输入**包的安全标识符 (SID)**和您的**密钥**，如下所示︰

    ![][36]

##<a id="send"></a>将通知发送到您的应用程序

我们现在将创建简单的推式通知市场到我们的应用程序发送推式通知。

1. 导航到移动服务门户中的**到达**选项卡。

2. 单击**新通知**来创建推式通知市场。
![][37]

3. 设置市场活动执行下面的步骤的第一个字段︰
![][38]

    一。 市场活动的名称。

    b。 为允许应用程序接收的通知是否或不启动该应用程序的*任何时候*选择**交货时间**。

    c。 在通知文本-键入**标题**中将推中以粗体显示。

    d。 然后键入您的消息。

4. 向下滚动，然后在**内容**部分中，选择**仅通知**。
![][39]

5. 在完成设置最基本的市场活动的可能。 现在再次向下滚动，然后单击**创建**按钮以保存您的市场活动。

6. 最后步骤︰ 单击**激活**来激活您的市场活动并发送推式通知。
![][41]

现在应该可以看到 toast 通知您的客户活动从您的设备上-应关闭该应用程序来查看此 toast 通知。 如果应用程序正在运行，请确保您有它的几分钟之前激活的市场活动，以便能够接收 toast 通知已结束。 如果您要集成在应用程序的通知，以便通知显示在应用程序打开时，请参阅[Windows 通用应用程序的覆盖集成]。

<!-- URLs. -->
[移动服务窗口通用 SDK 文档]: ../mobile-engagement-windows-store-integrate-engagement/
[移动服务窗口通用 SDK]: http://go.microsoft.com/?linkid=9864592
[Windows 商店开发人员中心]: http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409
[Windows 通用应用程序的覆盖集成]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/create-mobile-engagement-app.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/create-azme-popup.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/app-main-page-select-connection-info.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/app-connection-info-page.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/UniversalAppCreation.png
[20]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[21]: ./media/mobile-engagement-windows-store-get-started/manifest-declarations.png
[22]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[23]: ./media/mobile-engagement-windows-store-dotnet-get-started/subclass-page.png
[26]: ./media/mobile-engagement-windows-store-dotnet-get-started/engage-button.png
[27]: ./media/mobile-engagement-common/engagement-portal.png
[30]: ./media/mobile-engagement-windows-store-dotnet-get-started/clic-monitor-tab.png
[33]: ./media/mobile-engagement-windows-store-dotnet-get-started/monitor.png
[34]: ./media/mobile-engagement-windows-store-get-started/manifest-declarations-reach.png
[35]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[36]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[37]: ./media/mobile-engagement-windows-store-dotnet-get-started/new-announcement.png
[38]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-first-params.png
[39]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-content.png
[41]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-activate.png

测试
